---
title: 'Make file-picker modal in Chromium for Linux'
date: 2016-11-29T13:15:00.000-08:00
draft: false
aliases: [ "/2016/11/make-file-picker-modal-in-chromium-for.html" ]
---

When I stared working on Chromium, I found a bug in Chromium for Linux: the file-picker was not modal. So, while opening a file-picker, the users could control the main window. Sometimes, it caused mis-behaviors such as we were able to send an email while attaching a file.  
There was already [a bug](https://bugs.chromium.org/p/chromium/issues/detail?id=408481) and it seemed easy to fix it, but, it took 2 years to fully fix the problem.  
  
The root cause is as follows:  
  
Chromium for Linux uses GtkFileChooserDialog to open a file-picker, but it is not modal to the X11 host window because GtkFileChooserDialog can be modal only to the parent GtkWindow. So I tried to allow the X11 host window to disable input event handling to make a file-picker modal:  Here is the details:  

Opening a file-picker
---------------------

![](https://lh6.googleusercontent.com/deldsoZkd_koKpVHQMkIp6MFYpCIBPHJZlAMTQsPNXOsXwMfVO4_gTNP5R_WHWKmrK2Q2kkLNYZKah3BD1ueHtOnjw7NlDK5R05q3KplTPeYECT-sqPfUm5d0N6k90eWhnmSbMzY)

  
  
DisableEventListening() disables event listening for the host window using aura::ScopedWindowTargeter, which allows to temporarily replace the event-targeter with ui::NullEventTargeter. It returns a scope handle that is used to call |destroy\_callback| when closing the file-picker.  

  

class ScopedHandle {  
 public:  
   explicit ScopedHandle(const base::Closure& destroy\_callback);  
   ~ScopedHandle();  
   void CancelCallback();  
  
 private:  
    base::Closure destroy\_callback\_;  
    DISALLOW\_COPY\_AND\_ASSIGN(ScopedHandle);  
};

  
In addition, we also set another destroy callback(OnFilePickerDestroy) to the GtkFileChooserDialog that can be called when the file-picker is closed.  

Close the file-picker
---------------------

![](https://lh3.googleusercontent.com/1dcXzpAM5KvIEPZ9AQQalrNa7FOmA5swv6INlICgGfaS19mglnoKmBnI9EVYDpGAxkyVw1Ovhzei3tspqqVP8-6vZOXsm47uh-sXWxrsqSM4aKT1j0dCBWSdzZ6AFaSbv5ue5uJQ)

As you can see, OnFilePickerDestroy deletes scoped\_handle.  

void OnFilePickerDestroy(views::DesktopWindowTreeHostX11::ScopedHandle\*

   scoped\_handle) {

 delete scoped\_handle;

}

  
Then, |destroy\_callback| of ScopedHandle below is automatically called.  

void DesktopWindowTreeHostX11::EnableEventListening() {

 DCHECK(modal\_dialog\_xid\_);

 modal\_dialog\_xid\_ = 0;

 targeter\_for\_modal\_.reset();

}

  
You can find more details and discussion at [here](https://docs.google.com/document/d/12CfKVTpaonxxM3sNksq6vY6qb0J2qR3b7h_bLxzYanE/edit#).  
  

[The first change list was reverted](https://codereview.chromium.org/1594973009) due to the UI freezing problem that happens when the users open a file-picker from a child window of the X11 host window. [The second change list](https://codereview.chromium.org/1624793002/) finally fixed this issue(BUG 408481, 579408). I also added a test case for the fix: [BrowserSelectFileDialogTest.ModalTest](https://cs.chromium.org/chromium/src/chrome/browser/ui/libgtkui/select_file_dialog_interactive_uitest.cc?l=74&ct=xref_jump_to_def&gsn=MAYBE_ModalTest).