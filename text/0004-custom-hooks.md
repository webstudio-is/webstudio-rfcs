- Start Date: 2024-04-13
- RFC PR: (leave this empty)
- Issue: (leave this empty)

# Summary

Custom hooks are too powerful and need to be restricted to a set of principles when to use them.

# Basic example

Often custom hooks are used as a way to encapsulate any logic to either simplify a component or to share the logic between components. This is not a good use of custom hooks.

## Bad

Here opening a menu causes rerender of the text field component in an non-obvious way, especially when you don't see the implementation detail of useMenu at a glance.

```tsx
const useMenu = () => {
  const [isOpen, setIsOpen] = useState(false);
  return isOpen ? <...> : undefined;
}

const MyTextField1 = () => {
  const menu = useMenu();
  return (
    <div>
      <input />
      {menu}
    </div>
  );
};
const MyTextField1 = () => {
  const menu = useMenu();
  return (
    <div>
      <input />
      {menu}
    </div>
  );
};
```

# Motivation

The reason why that example above might be not apparent at a first glance, because there is nothing wrong technically about sharing some logic via a custom hook, the realization comes later when you have a set of issues:

1. Custom hooks are not pure functions. They have ability to rerender the component and cause side effects. While this is generally very powerful, this shouldn't be used outside of the component itself, because it hides these details and unfortunately its very important to see them when reading the component. Moving logic to a custom hook just moves the problem to a different place, hiding it instead fo fixing it.

2. Hooks make it too easy to create a convoluted abstraction. At the moment of writing a hook you think you know everything about the requirements, then you start using that hook in multiple places and then you extend it and now you are in a mess. The core issue here is that creating stateless shared abstractions is already hard enough, but creating a stateful shared abstraction is even harder. A component is a better interface for shared abstraction.

# Detailed design

1. Custom hooks should only be created for very generic use cases that are not coupled to specific component usage or application features. They need to be well written and documented.
2. Anything that can cause component rerender or side effect should be clearly written inside the component, not in a custom hook.
3. If component is too complex it should be split into smaller components, not into custom hooks.

## Good

By extracting the shared logic into a component, changing open state doesn't cause MyTextField rerender.

```tsx
const Menu = () => {
  const [isOpen, setIsOpen] = useState(false);
  return isOpen ? <...> : undefined;
};
const MyTextField = () => {
  return (
    <div>
      <input />
      <Menu />
    </div>
  );
};

```

# Drawbacks

1. It may feel inflexible to not use custom hooks for sharing logic.

# Alternatives

Different component models like the one in Solid, but we can't switch from React.

# Adoption strategy

Incremental adoption as we move forward.
