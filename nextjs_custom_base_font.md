### NextJS 에서 기본 폰트 변경하기

새로운 프로젝트를 진행하는데 있어서 기본 설정 폰트가 아닌 Pretendard 폰트를 사용하게 되어서 해당 폰트로 변경하면서 알게된 내용을 정리한다.

먼저, Pretendard 폰트는 [여기](https://cactus.tistory.com/306) 에서 다운받을 수 있다.

폰트를 준비한 후에 NextJS 프로젝트 내에서 기본 폰트를 변경하기 위해 아래 링크의 문서를 참고하였다.

https://nextjs.org/docs/app/building-your-application/optimizing/fonts#local-fonts

google font 가 아니기 때문에 `next/font/local` 을 이용해서 로컬에 있는 폰트 파일을 지정하면 되는 데 이때 performance 와 flexibility 를 위해서
`variable font` 를 사용하라고 되어 있다.

근데 variable font 는 뭐지? 이와 관련된 자세한 내용은 [여기](https://namu.wiki/w/가변%20글꼴)에서 확인할 수 있는데 요약하면 아래와 같다.

> 어떤 기존 글꼴 파일의 패밀리가 두께에 따라 Thin, ExtraLight, Light, Regular, Medium, SemiBold, Bold, ExtraBold, Black의 총 9가지 파일을 따로 만들었다고 하자. 게다가 여기서 너비에 따라 Condensed, SemiCondensed까지 추가하면 총 27종을 만들어야 한다. 하지만 가변 글꼴로 만든다면 하나의 글꼴 파일에서 두께와 너비라는 축을 만들고, 그 축을 기준으로 가장 가는 서체에서 서서히 굵은 서체로, 가장 좁은 서체에서 넓은 서체로 변하는 알고리즘을 적용한다.
>
> 따라서 가변 글꼴 파일의 용량은, 물론 1개 글꼴 파일보다는 크지만, 적어도 굵고 가늘고 넓고 좁은 4개 글꼴 파일의 용량보다는 작거나 비슷한 정도로 위의 27개보다 훨씬 많은 글꼴 모양을 표현할 수 있게 된다. 이에 따라 웹 폰트로 사용할 때, 많은 폰트 패밀리를 필요로 한다면, 가변 글꼴 파일의 수가 적어서 브라우저의 글꼴 파일 요청 횟수가 줄기 때문에 기존의 방식으로 많은 글꼴을 불러오는 것보다 성능 향상에도 도움을 준다.

다행히 Pretendard 에서도 가변 폰트를 제공하고 있어서 아래과 같이 적용하면 된다.

```
import localFont from 'next/font/local'
 
// Font files can be colocated inside of `app`
const myFont = localFont({
  src: './fonts/PretendardVariable.woff2',
  display: 'swap',
  weight: '45 920',
})
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body className={myFont.className}>{children}</body>
    </html>
  )
}
```

그런데 새 프로젝트에서 NextJS 와 TailwindCSS 를 같이 쓰고 있어서 한 가지 추가로 해야 할 것이 있다.

[여기](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#with-tailwind-css)에서 언급한 것처럼 폰트를 로드할 때 variable을 설정하고 이걸 body 에 추가하도록 한다 추가로 tailwind.config.js 에 font-family 설정을 하면 된다.

```
import type { Config } from "tailwindcss";
import defaultTheme from "tailwindcss/defaultTheme";

const config: Config = {
  ...
  theme: {
    extend: {
      fontFamily: {
        sans: ["var(--font-pretendard)", ...defaultTheme.fontFamily.sans],
      },
    },
  },
  plugins: [],
};
export default config;
```

여기에서의 내용은 tailwindcss 를 추가로 참고하자. `font-sans` 에 pretendard 를 가장 앞에 두고 기본으로 가지고 있는 것들을 붙여주도록 하려면 defaultTheme 로 부터 sans 내용을 꺼내와야 한다.



