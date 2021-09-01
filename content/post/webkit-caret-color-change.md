---
title: 'Caret should respect text background color'
date: 2021-08-28T22:12:00.001-07:00
draft: false
aliases: [ "/27/08/2021-webkit-caret-color-change.html" ]
tags : [WebKit, Contribution]
---
## History
[I fixed a caret color issue in WebKit in 2013](https://trac.webkit.org/changeset/152612/webkit), but I found out there was a regression, which made me work this issue again and finally fixed it. o/
It was a bit hard for me because I had not worked on WebKit since [Chromium forked from the WebKit project in 2013](https://techcrunch.com/2013/04/03/google-forks-webkit-and-launches-blink-its-own-rendering-engine-that-will-soon-power-chrome-and-chromeos).

The source code has been changed and I almost forgot how to deal with layout test errors.
My WebKit debugging skill is also rusty. I tried to remind how to work on a patch by reading my own documents and old ccmmits.

## Change the code
Here is a breif overview:

When I first fixed this issue, I just updated one line of code as follows:
```
+++ b/trunk/Source/WebCore/editing/FrameSelection.cpp
@@ -1470,5 +1470,6 @@
     Color caretColor = Color::black;
     ColorSpace colorSpace = ColorSpaceDeviceRGB;
-    Element* element = node->rootEditableElement();
+    Element* element = node->isElementNode() ? toElement(node) : node->parentElement();
+
     if (element && element->renderer()) {
         caretColor = element->renderer()->style()->visitedDependentColor(CSSPropertyColor);
```

My patch allowed WebKit to get the caret color from the element containing the text, not the root editable element.

```
void CaretBase::paintCaret(Node* node, GraphicsContext* context, const LayoutPoint& paintOffset, const LayoutRect& clipRect) const
{
#if ENABLE(TEXT_CARET)
    if (m_caretVisibility == Hidden)
        return;


    LayoutRect drawingRect = localCaretRectWithoutUpdate();
    RenderObject* renderer = caretRenderer(node);
    if (renderer && renderer->isBox())
        toRenderBox(renderer)->flipForWritingMode(drawingRect);
    drawingRect.moveBy(roundedIntPoint(paintOffset));
    LayoutRect caret = intersection(drawingRect, clipRect);
    if (caret.isEmpty())
        return;


    Color caretColor = Color::black;
    ColorSpace colorSpace = ColorSpaceDeviceRGB;
    Element* element = node->isElementNode() ? toElement(node) : node->parentElement();


    if (element && element->renderer()) {
        caretColor = element->renderer()->style()->visitedDependentColor(CSSPropertyColor);
        colorSpace = element->renderer()->style()->colorSpace();
    }


    context->fillRect(caret, caretColor, colorSpace);
#else
    UNUSED_PARAM(node);
    UNUSED_PARAM(context);
    UNUSED_PARAM(paintOffset);
    UNUSED_PARAM(clipRect);
#endif
}

Index: trunk/Source/WebCore/editing/FrameSelection.cpp
===================================================================
--- a/trunk/Source/WebCore/editing/FrameSelection.cpp
```


After 8 years, the mainline code has been changed as follows:

```
Color CaretBase::computeCaretColor(const RenderStyle& elementStyle, const Node* node)
{
    // On iOS, we want to fall back to the tintColor, and only override if CSS has explicitly specified a custom color.
#if PLATFORM(IOS_FAMILY) && !PLATFORM(MACCATALYST)
    UNUSED_PARAM(node);
    return elementStyle.caretColor();
#else
    RefPtr parentElement = node ? node->parentElement() : nullptr;
    auto* parentStyle = parentElement && parentElement->renderer() ? &parentElement->renderer()->style() : nullptr;
    // CSS value "auto" is treated as an invalid color.
    if (!elementStyle.caretColor().isValid() && parentStyle) {
        auto parentBackgroundColor = parentStyle->visitedDependentColorWithColorFilter(CSSPropertyBackgroundColor);
        auto elementBackgroundColor = elementStyle.visitedDependentColorWithColorFilter(CSSPropertyBackgroundColor);
        auto disappearsIntoBackground = blendSourceOver(parentBackgroundColor, elementBackgroundColor) == parentBackgroundColor;
        if (disappearsIntoBackground)
            return parentStyle->visitedDependentColorWithColorFilter(CSSPropertyCaretColor);
    }
    return elementStyle.visitedDependentColorWithColorFilter(CSSPropertyCaretColor);
#endif
}
```


CaretBase::computeCaretColor() is separated from CaretBase::patinCaret(). C++ 11 features and smart pointer are used (https://webkit.org/blog/3172/webkit-and-cxx11/)

Here is my change:
```
Index: trunk/Source/WebCore/editing/FrameSelection.cpp
===================================================================
--- a/trunk/Source/WebCore/editing/FrameSelection.cpp
+++ b/trunk/Source/WebCore/editing/FrameSelection.cpp
@@ -1791,13 +1791,13 @@
     return elementStyle.caretColor();
 #else
-    auto* rootEditableElement = node ? node->rootEditableElement() : nullptr;
-    auto* rootEditableStyle = rootEditableElement && rootEditableElement->renderer() ? &rootEditableElement->renderer()->style() : nullptr;
+    RefPtr parentElement = node ? node->parentElement() : nullptr;
+    auto* parentStyle = parentElement && parentElement->renderer() ? &parentElement->renderer()->style() : nullptr;
     // CSS value "auto" is treated as an invalid color.
-    if (!elementStyle.caretColor().isValid() && rootEditableStyle) {
-        auto rootEditableBackgroundColor = rootEditableStyle->visitedDependentColorWithColorFilter(CSSPropertyBackgroundColor);
+    if (!elementStyle.caretColor().isValid() && parentStyle) {
+        auto parentBackgroundColor = parentStyle->visitedDependentColorWithColorFilter(CSSPropertyBackgroundColor);
         auto elementBackgroundColor = elementStyle.visitedDependentColorWithColorFilter(CSSPropertyBackgroundColor);
-        auto disappearsIntoBackground = blendSourceOver(rootEditableBackgroundColor, elementBackgroundColor) == rootEditableBackgroundColor;
+        auto disappearsIntoBackground = blendSourceOver(parentBackgroundColor, elementBackgroundColor) == parentBackgroundColor;
         if (disappearsIntoBackground)
-            return rootEditableStyle->visitedDependentColorWithColorFilter(CSSPropertyCaretColor);
+            return parentStyle->visitedDependentColorWithColorFilter(CSSPropertyCaretColor);
     }
     return elementStyle.visitedDependentColorWithColorFilter(CSSPropertyCaretColor);
```
One of the reviewer asked me to use RefPtr(smart pointer) instead of auto so I changed this line as follows:
```
auto parentElement = node ? node->parentElement() : nullptr;
```
=>
```
RefPtr parentElement = node ? node->parentElement() : nullptr;
```

[RefPtr](https://webkit.org/blog/5381/refptr-basics/) is a class template that implements WebKit’s intrusive reference counting. It has to be used instead of raw pointers when contributors update the source code.


## Steps to contribution of your code
Below are the steps I followed. You can find more details at https://webkit.org/contributing-code/

Build WebKit
```
$  Tools/Scripts/build-webkit
```
You don’t need to run every test case. Instead, you can only run the relevant layout tests as follows:

Layout test
```
$ Tools/Scripts/run-webkit-tests editing/caret
```
If you need pixel tests, just pass -p

Sometimes, we need to update ChangeLog
```
$ Tools/Scripts/prepare-ChangeLog --g HEAD
```
Finally, you can upload your patch:
```
$ Tools/Scripts/webkit-patch upload 117493
```
It would be good to add a message to your updated patch:
```
$ Tools/Scripts/webkit-patch upload 117493 -m “Updated ChnageLog”
```

When your patch lands, “Reviewed by NOBODY (OOPS!)” line in ChangeLog is automatically filled with the name of the reviewer who approves your patch by setting review:+ and pushes it to the commit queue(AFAIK).

https://ews-build.webkit.org/#/


In my case, I had to add the reviewer name because I updated the patch several times after getting +r.

If you change the layout code and then there would be multiple layout test failures: 
![source diff](/images/caret_color_diff.png)
![actual result of caret color layout test ](/images/caret_color_acutal.png)


If you find any layout test errors, you can easily get *-actual.txt from the layout test result. Just overwrite *-expcted.txt with the *-actual.txt.

Then, add them to your patch and run webkit-patch command again. If you already have +R, just add your patch to the commit queue.

The committer status has been suspended due to my inactivity so I was not able to add my patch to the commit queue. 
However, one of the reviewers helped me land this patch. The next plan is to work on [Bug 44862 - Make the caret more visible on any background](https://bugs.webkit.org/show_bug.cgi?id=44862) again. 





