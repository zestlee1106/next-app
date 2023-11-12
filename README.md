# Next.js 13 맛보기

## Next.js 13 은 뭐가 다른가?

> 사실 12 도 잘 몰라서 모르겠지만... 일단 써보도록 한다

1. 최신 버전은 app 디렉토리를 쓴다고 한다.
2. src 가 없는 형태로 쓴다.
3. nuxt 에서의 index.vue 처럼, 페이지는 page.tsx 로 만들어야 한다. (이렇게 해야 라우팅을 저절로 만들어 주는 듯)
4. public 에 만들어 줘야 퍼블릭한 경로로 접근이 가능하다
5. Next 에서 링크 이동을 할 땐 a 태그보다는 Next 의 Link 컴포넌트를 써야 한다
   1. 이것은 Nuxt 에서도 동일한데, 일단 제공해주는 컴포넌트를 써야 프리페칭이 가능하고
   2. a 링크를 쓰면 이동할 때마다 페이지를 렌더링하기 위해서 필요한 네비게이션이나 헤더 푸터를 다시 가져오기 때문에, 필요없는 네트워크 리소스 낭비가 되기 쉽다
6. 서버에서 렌더링 되는 대로 페이지를 확인하고 싶다면 네트워크 탭에서 html 을 확인해 보자
7. 일반 components 디렉토리에 컴포넌트를 만들 경우 server 컴포넌트이기 때문에, 맨 위에 `'use client'` 라는 걸 달아 줘야 클라이언트에서만 해당 컴포넌트가 동작한다
   1. 클릭 이벤트 등 클라이언트에서만 동작하는 것들이 있다면, 클라이언트 전용 컴포넌트로 만들어 줘야 한다
   2. 그렇다고 전체 컴포넌트를 클라이언트로 만들 필요 없이, 필요한 부분만 추출해내어 클라이언트 컴포넌트로 만들어 줘야 한다. (그것이 진짜 컴포넌트로 잘 찢는 플젝)
8. 서버 컴포넌트에서 그냥 fetch 를 호출하면, 그 데이터 페칭은 서버에서 호출된다. (Nuxt 의 asyncData 와 같다)

### 캐싱

- Next.js 에서 fetch 를 쓸 때 데이터 캐싱을 쓸 수 있다
- 이것은 fetch 를 호출할 때 뒤에 object 로 붙여줘야 한다

```js
fetch("https://jsonplaceholder.typicode.com/users", {
  cache: "no-store",
});
```

- 그리고 또한 Next.js 에서는 revalidate 라는 기능이 존재했는데, 원래는 getStaticProps 같은 함수로 넘겨줘야 했으나, 이제 fetch 옵션에 추가만 해 주면 된다

```js
fetch("https://jsonplaceholder.typicode.com/users", {
  next: { revalidate: 10 },
});
```

**번외) revalidate 가 무엇인가?**

- SSG 로 페이지를 만들 경우, 사용자는 한번 데이터 페칭이 된 데이터만 계속 보게 된다
- 그럼 새로 빌드가 되기 전까지 1년이 된 데이터, 10년이 된 데이터만 주구장창 볼 수도 있다
- Next 에서는 그것을 방지하기 위해 revalidate 라는 옵션을 만들어놨으며, build 와 generate 시간을 비교하여 trigger 를 발생시켜 다시 date fetching 을 시킨다

**번외2) daisyui**

- [daisyui](https://daisyui.com/) 라는 라이브러리가 있는데, 이것을 설치하고 tailwind 플러그인에 등록하면 부트스트랩처럼 사용할 수 있다
- eliment ui 처럼 클래스에 선언하는 것만으로도 해당 컴포넌트를 사용할 수 있기 때문에 컴포넌트를 import 할 필요가 없어서 훨씬 편하다
- 테마도 설정할 수 있으니 필요하다면 도큐먼트에 가서 보는 것도 좋을 듯하다

---

이것은 [Mosh](https://youtu.be/ZVnjOPwW4ZA?si=VDXk7Txts6hcgguo) 의 튜토리얼 영상을 보고 했는데, 이 개발자의 유튜브가 꽤 쏠쏠하다
