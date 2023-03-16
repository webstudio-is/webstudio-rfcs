- Start Date:  2023-03-16
- RFC PR: (leave this empty)
- Issue: (leave this empty)

# Summary

We need a keyboard accessibility standard for Webstudio, since global standards only provide recommendations in many cases, but we need to be consistent.

# Detailed design

## Which elements should be focusable?

All interactive elements should be focusable by keyboard. Interactive elements are those which do anything user-facing on mouse or keyboard interactions like click or enter.

Example: button or a link should always be focusable, where div that has no interactive usage should never be focusable.

## Vertical lists of focusable items

Long lists with focusable items are hard to exit if we tab through items. This is why once the first list element is focused, we choose to use arrow keys to navigate focus in the list.
Tab will focus next focusable element after the list

## Nested focusable elements

When you have an item which is focusable/interactive and then inside of that item you have more interactive elements, we have a choice to make.
We can either let user tab into the child elements or allow using arrow keys as a secondary mechanism to focus child elements.
For consistency with lists, we choose to use arrow keys for all child elements and tab for focusing the next focusable element that comes after the parent.

## Vertical list focusable items with nested focusable elements

In this case we already decided to use vertical arrow keys to focus list items. To focus list item focusable child elements, we now can only use horizontal arrow keys.

## Two-dimensional lists

When list's layout is 2-dimensional, vertical arrow keys should shift to the next vertical element, while horizontal arrow keys should shift horizontal elements.

## Loops

When first element is focused going to the previous one should focus the last one and vice versa. This way we can go in the loop using arrow keys.

# Drawbacks

- Wome may feel that using tab to focus a list item is more intuitive due to the past experience. We don't support that to provide a consistent way to escape the list quickly.

# Alternatives

Lots of alternatives, but the point of this RFC is to have a consistent good way to handle all scenarios for a complex UI 

# Unresolved questions

Unknown