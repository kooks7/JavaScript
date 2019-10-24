---
layout: post
title:  "Sync Async"
categories : Coding!
date:   2019-10-24



---

## sync

```javascript
function foo() {
  console.log("Hello!");
}
function bar() {
  foo();
  console.log("Bye!");
}
function hoo() {
  bar();
  console.log("Help!");
}
console.log("Start!");
hoo();
console.log("End!");

//Start!
//Hello!
//Bye!
//Help!
//End!

```

## Async

```javascript
console.log("Hello!");
setTimeout(() => {
  console.log("API를 찔렀어요");
}, 2000);
setTimeout(() => {
  console.log("두번째로 API를 찔렀어요");
}, 2000);
console.log("End");
// hello 2초 후 API를 찔렀어요 End
// hello end 2초후에 API
```

## promise

```javascript
const promise = new Promise((resolve, reject) => {
  // Async 작업...
  const data = [1, 2, 3, 4, 5];
  resolve(data);
  reject("실패했어요ㅠㅜ");
});

// promise
//   .then(result => console.log(result))
//   .catch(error => console.log(error));

// const getAccount = new Promise(
//   (resolve, reject) => {
//     setTimeout(() => {
//       const number = Math.floor(
//         Math.random() * 100,
//       );
//       // 0 ~ 99까지의 수를 랜덤하게 뽑아냅니다.
//       if (number % 2 === 1)
//         resolve({ id: 1, balance: 1000 });
//       else
//         reject(
//           new Error(
//             "계좌에 접근할 수 없어요ㅠㅠ",
//           ),
//         );
//     }, 2000);
//   },
// );

// getAccount
//   .then(result => console.log(result))
//   .catch(err => console.error(err));

function getUser(id) {
  console.log("유저를 찾고 있습니다...");
  const users = [{ id: 1, githubID: "bing" }, { id: 2, githubID: "kim" }];
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const user = users.find(user => user.id === id);
      if (user) resolve(user);
      else reject(new Error("유저를 찾을 수 없네요"));
    }, 2000);
  });
}

function getRepo(githubID) {
  console.log("Github 리포를 찾는 중입니다...");
  const repos = [
    { githubID: "bing", commitID: 1 },
    { githubID: "kim", commitID: 2 }
  ];
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const repo = repos.find(repo => repo.githubID === githubID);
      if (repo) resolve(repo);
      else reject(new Error("리포를 찾을 수 없어요..."));
    }, 2000);
  });
}
function getCommit(commitID) {
  console.log("커밋을 찾는 중입니다...");
  const commits = [
    { commitID: 1, contents: "첫번째 커밋" },
    { commitID: 2, contents: "두번째 커밋" }
  ];
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const commit = commits.find(commit => commit.commitID === commitID);
      if (commit) resolve(commit);
      else reject(new Error("커밋을 찾을 수 없어요"));
    }, 2000);
  });
}
getUser(1)
  .then(user => getRepo(user.githubID))
  .then(repo => getCommit(repo.commitID))
  .then(commit => console.log(commit))
  .catch(err => console.error(err));
//어렵네...

```

## async-await

```javascript
const user = await getUser(1);
const repo = await getRepo(user);
const commit = await getCommit(repo.commitID);
console.log(commit);

function getUser(id) {
  console.log("유저를 찾고 있습니다...");
  const users = [{ id: 1, githubID: "bing" }, { id: 2, githubID: "kim" }];
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const user = users.find(user => user.id === id);
      if (user) resolve(user);
      else reject(new Error("유저를 찾을 수 없네요"));
    }, 2000);
  });
}

function getRepo(githubID) {
  console.log("Github 리포를 찾는 중입니다...");
  const repos = [
    { githubID: "bing", commitID: 1 },
    { githubID: "kim", commitID: 2 }
  ];
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const repo = repos.find(repo => repo.githubID === githubID);
      if (repo) resolve(repo);
      else reject(new Error("리포를 찾을 수 없어요..."));
    }, 2000);
  });
}
function getCommit(commitID) {
  console.log("커밋을 찾는 중입니다...");
  const commits = [
    { commitID: 1, contents: "첫번째 커밋" },
    { commitID: 2, contents: "두번째 커밋" }
  ];
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const commit = commits.find(commit => commit.commitID === commitID);
      if (commit) resolve(commit);
      else reject(new Error("커밋을 찾을 수 없어요"));
    }, 2000);
  });
}
getUser(1)
  .then(user => getRepo(user.githubID))
  .then(repo => getCommit(repo.commitID))
  .then(commit => console.log(commit))
  .catch(err => console.error(err));

```

` 콜백 => promise 로 변환`

```javascript
// getCustomer(1, customer => {
//   console.log("Customer: ", customer);
//   if (customer.isGold) {
//     getTopMovies(movies => {
//       console.log("Top movies: ", movies);
//       sendEmail(customer.email, movies, () => {
//         console.log("Email sent...");
//       });
//     });
//   }
// });
//1. 실행

(async function() {
  const customer = await getCustomer(1);
  console.log("Customer: ", customer);
  if (customer.isGold) {
    const topMovies = await getTopMovies();
    console.log("Top moives:", topMovies);
    await sendEmail(customer.email, movies);
    console.log("Email send...");
  }
})();

// function getCustomer(id, callback) {
//   setTimeout(() => {
//     callback({
//       id: 1,
//       name: "Mosh Hamedani",
//       isGold: true,
//       email: "email"
//     });
//   }, 4000);
// }
//2. 
function getCustomer(id) {
  return new Promise(resolve => {
    setTimeout(() => {
      callback({
        id: 1,
        name: "Mosh Hamedani",
        isGold: true,
        email: "email"
      });
    }, 2000);
  });
}

// function getTopMovies() {
//   setTimeout(() => {
//     resolve(["movie1", "movie2"]);
//   }, 4000);
// }
//3.
function getTopMovies() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(["movie1", "movie2"]);
    }, 2000);
  });
}

// function sendEmail(email, movies, callback) {
//   setTimeout(() => {
//     callback();
//   }, 4000);
// }
//4.
function sendEmail(email, movies) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve();
    }, 2000);
  });
}

```

