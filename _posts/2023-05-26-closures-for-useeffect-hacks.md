---
layout: post
title: closures for useEffect hacks
date: 2023-05-26 12:05 +0800
categories: [design]
tags: [typescript, react]
---

Have you ever gotten into a scenario where you tried to nest a useEffect hook within a useEffect hook? that is not allowed in React as it has to be at the top level of a functional component, it needs the order and consistency of hooks to manage the component state, thats why conditions and loops are also not allowed.

but there tends to be a need to just do a useQuery hook to call an endpoint. Unfortunately useQuery under the hood uses a useEffect hook and hence u cant just call the endpoint.

A work around is to use closures, which preseves the state of the scope.
Closures are basically a function that contains a inner function that uses its outside scope.

```typescript
function outerFunction() {
  const outerVariable = 'hello';

  function innerFunction() {
    console.log(outerVariable);
  }
  return innerFunction
}
```

But how does this work in useEffect?

a simple example is as such, we will use useMutation of the same useQuery library that i recently ran into:

```typescript
export const useUpdateBulkUser = () => {
  const updateData = api.stockTakeData.bulktransactionUpdateData.useMutation();
  
  //closure function
  const updatebulkUser = async (value: UpdateDBField[]) => {
    console.log(value);

    const transaction = value.map((row: UpdateDBField) => {
      return { where: { id: row.id }, data: { [row.field]: row.fieldValue } };
    });
    await updateData.mutateAsync(transaction);
  };

  return updatebulkUser;
};
```

In this case, the mutation is wrapped into a closure function that calls the outside useMutation hook but returns the inner function, i can now use this useUpdateBulkUser inside my useEffect!

