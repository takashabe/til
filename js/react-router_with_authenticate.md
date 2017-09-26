## react-router v4でRouteコンポーネントに任意のpropsを注入する

```jsx
const renderMergedProps = (component, ...rest) => {
  const finalProps = Object.assign({}, ...rest);
  return (
    React.createElement(component, finalProps)
  );
}

const PropsRoute = ({ component, ...rest }) => {
  return (
    <Route {...rest} render={routeProps => {
      return renderMergedProps(component, routeProps, rest);
    }}/>
  );
}

<Router>
    <Switch>
      <PropsRoute path='/login' component={Login} auth={auth} authenticatedRedirect="/" />
      <PropsRoute path='/allbooks' component={Books} booksGetter={getAllBooks} />
      <PropsRoute path='/mybooks' component={Books} booksGetter={getMyBooks} />
      <PropsRoute path='/trades' component={Trades} user={user} />
    </Switch>
</Router>
```

https://github.com/ReactTraining/react-router/issues/4105#issuecomment-289195202
