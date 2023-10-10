- Start Date: 10-10-2023
- RFC PR: (leave this empty)
- Issue: (leave this empty)

# Summary

Describes how we use CSS custom properties or CSS variables in the builder.

# Motivation

1. Avoid namespace collisions
2. Avoid copy-pasting CSS variable names
3. Provide a way to define a typed interface for CSS variables

CSS variables are a subject to inheritance, hence in a large app you can get conflicts. In addition to that not all values should be allowed and we should be able to trace what code is setting a value.
By defining a convention and using a TS interface we can solve both problems.

# Detailed design

1. Avoiding namespace collision is solved by using a naming convention: `--ws-{component}-{var}` where `component` is the component name that is going to render the variable and `var` is the variable namespace.
2.  We never copy-paste a CSS variable name across modules. When variable needs to be defined outisde of a module, we decided to export a typed function
    Example 1:
    ```ts
    export const setAmplitude = (value: number) => ({
      "--ws-ai-command-bar-amplitude": value,
    });
    ```

    Example 2:
    ```ts
    export const setCommandBarVars = ({amplitude, position}: {amplitude: number, position: 'top' | 'bottom'}) => ({
      "--ws-ai-command-bar-amplitude": amplitude,
      "--ws-ai-command-bar-position": position,
    });
    ```
    
# Drawbacks

- more typing than simply using vars without any convention
- enforcing convention        

# Alternatives

- not using any conventions
- using a js utility function
