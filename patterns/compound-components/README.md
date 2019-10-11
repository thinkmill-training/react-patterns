## Compound components

```JSX
<ButtonGroup>
  <Button>First</Button>
  <Button>Second</Button>
  <Button>Last</Button>
</ButtonGroup>
```

- [ ] What is the strategy around providing great ergonomics, so consumer doesn't need to worry about decorating the children component with props like `first`, `last` etc.

  - Iterating over `React.Children` and attaching props?
  - Context API (allows to access the props beyond first-level children)
  - Do we have a set of _decision-making rules_ for that?

---

- [ ] Would we ever bother using the JSX _dot notation_ for compound components? As in:

```JSX
<Panel>
  <Panel.Title>Well, hi!</Panel.Title>
  <Panel.Body>
    Hello, Friend. I hope you're well!
  </Panel.Body>
  <Panel.Footer>
    Have a good day!
  </Panel.Footer>
</Panel>
```
