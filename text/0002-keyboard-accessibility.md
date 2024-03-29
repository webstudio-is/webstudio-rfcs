- Start Date: March 16, 2023
- RFC PR: (leave this empty)
- Issue: (leave this empty)

# Summary

A quick summary of accessibility principles we use for Webstudio. Full guide is https://www.w3.org/WAI/ARIA/apg/patterns/

# Detailed design

## Which elements should be focusable?

All interactive elements should be focusable by keyboard. Interactive elements are those that do anything user-facing on mouse or keyboard interactions, like click or enter.

Example: A button or a link should always be focusable, whereas a div that has no interactive usage not be focusable, unless it is a list item and has focusable elements inside.

## Vertical lists of focusable items

Long lists with focusable items are hard to exit if we tab through items. This is why once the first list element is focused, we choose to use arrow keys to navigate focus in the list.
Tab will focus the next tabbable element after the list.

## Nested focusable elements

When you have an item that is focusable/interactive and then inside of that item you have more interactive elements, we have a choice to make.
We can either let the user tab into the child elements or allow using arrow keys as a secondary mechanism to focus child elements.
For consistency with lists, we choose to use arrow keys for all child elements and tab for focusing the next tabbable element that comes after the parent.

## Vertical list with focusable items with nested focusable elements

In this case, we have already decided to use vertical arrow keys to focus list items. To focus list item focusable child elements, we can now only use horizontal arrow keys.

## Grids

When a list's layout is 2-dimensional, vertical arrow keys should shift focus to the next vertical element, while horizontal arrow keys should shift focus of horizontal elements.

## Loops

When the first element is focused, going to the previous one should focus the last one and vice versa. This way, we can go in the loop using arrow keys.

# Drawbacks

Some may feel that using tab to focus a list item is more intuitive due to past experience. We don't support that to provide a consistent way to escape the list quickly.

# Alternatives

There are lots of alternatives, but the point of this RFC is to have a consistent way to handle all scenarios for a complex UI.

# Unresolved questions

Unknown
