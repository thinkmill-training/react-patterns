# Prop API Design

## Designing good props API

- [ ] What makes for a good props API design?
  - "bubbling up" events to allow other components to override them
  - allowing other component to override local state via a prop

## Interesting and Alternative Patterns

### Dynamic Types

Consider using dynamic types to minimize discrete props e.g.

```jsx
<Dialog width={[100, 300]} />
<Dialog width={240} />
// instead of
<Dialog minWidth={100} maxWidth={300} />
<Dialog width={240} />
```

Allow both `$React.Node` children and render functions to pass on relevant helpers e.g.

```jsx
<Popover target={<Button>Target</Button>}>
  Some Content
</Popover>
// OR
<Popover target={({ isOpen }) => (<Button selected={isOpen}>Target</Button>)}>
  Some Content
</Popover>
// implementation
let resolvedTarget = typeof target === 'function' ? target(state) : target
```

### Data VS Composition

Rather than making the decision for the consumer, allow various (appropriate) render methods

```jsx
<RadioGroup>
  <Radio value="one" label="One" />
  <Radio value="two" label="Two" />
</RadioGroup>
// OR
<RadioGroup data={[
  { value: "one", label: "One" },
  { value: "two", label: "Two" },
]} />
```

### Flexible Render/Styling

Make the most common usage easy, whilst allowing consumers to deviate when necessary; limiting adoption deterrents.

Take advantage of React's `isValidElement` helper.

```jsx
<Header title="Page One" />
<Header title={<Avatar src="/path/to/image.jpg" name="User Name" />} />
// implementation
let resolvedTitle = React.isValidElement(title) ? title : <Title>{title}</Title>
```

### Children Enhancement

Take advantage of React's `Children` helpers. One usage could be to avoid trumping consumer styles e.g.

```jsx
Children.map(props.children, child => (
  <div style={{ margin: 20 }}>
    {child}
  </div>
))
```

### Code Simplification

#### Common Utilities

Consolidate common behaviour with robust helper utilities to remove noise from your components.

```jsx
const handleClick = (event) => {
  if (props.onClick) {
    props.onClick(event);
  }
  ...
}
// instead
const handleClick = wrapHandlers(props.onClick, () => {
  ...
});
// implementation
export const wrapHandlers = (consumerHandler, ourHandler) => event => {
  if (typeof consumerHandler === 'function') {
    consumerHandler(event);
  }

  if (!event.defaultPrevented) {
    ourHandler(event);
  }
};
```

#### Simple Maps

Create a style/value map to avoid noisy ternaries and if statements e.g.

```jsx
const appearanceMap = {
  primary: { background: 'green', color: 'white' },
  secondary: { background: 'pink', color: 'wheat' }
}

const SomeComponent = ({ appearance }) => {
  const resolvedAppearance = appearanceMap[appearance]

  return <div css={resolvedAppearance} />
}
```
