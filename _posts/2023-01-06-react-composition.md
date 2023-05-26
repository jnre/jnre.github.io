---
layout: post
title: React composition
date: 2023-01-06 17:19 +0800
categories: [design]
tags: [typescript, react]
---

by default, usually we would use prop drilling to pass props to nested components for simple cases. however this gets very cluttered if there are many nested components and the drilling gets overly crowded. Moreover some middle layer components may not use the state that were passed through it, hence it is actually redundant.

alternatively, one way to do so is using context api, which allows you to wrap the providers. from the react website: `if you only want to avoid passing some props through many levels, component composition is often a simpler solution than context`. context api is more for when th data needs to be accessible by many components at different nesting levels.

how it works:

```typescript

const App = () => {
  const [currentUser, setCurrentUser] = useState("")

  return (
    <div>
      <Page title="welcome">
        <DashboardNav />
        <Dashboard user={currentUser} />
      </Page>
    </div>
  )
}

type PageProps = {
  title: string;
  children?: React.ReactNode;
};

const Page = ({ title, children }: PageProps) => (
  <div>
    <h1>{title}</h1>
    {children}
  </div>
);

type DashBoardProps = {
  user: string;
  children?: React.ReactNode;
}

const Dashboard = ({ user,children }: DashBoardProps)=> { //making it optional children
  return (
    <div>
      <h2>{user}</h2>
      {children}
    </div>
  )
}

const DashboardNav = () => {
  return (
    <div>
      <h3>Dashboard Nav</h3>
    </div>
  )
}

```