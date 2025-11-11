componentDidMount → useEffect(func, [])

componentWillUnmount → useEffect(() => func, [])

componentDidUpdate → useEffect(func, [props])

There were some sacrifices to performance brought out by this movement - its bandages visible as the Hooks useMemo and useCallback. I don’t mean to imply that memoization did not predate Hooks in React. It did (React.memo()). I’m saying we now have to memoize state initialization and transformations due to the improvements we’ve made in localizing behavior.