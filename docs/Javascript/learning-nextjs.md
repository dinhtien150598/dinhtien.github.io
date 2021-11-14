---
title: Learning NextJs
sidebar_position: 3
---

# Cùng tìm hiểu về NextJs

## NextJs là gì?

Theo như trang chủ của NextJs thì nó là một React Framework cho Production (có host sẵn cho việc deploy).

Next.js sẽ cung cấp cho người lập trình viên những tính năng trải nghiệm cần cho production: `hybird static` ,`server rendering`, `Typescript support`, và nhiều chức năng khác xịn xò. Nó không yêu cầu cấu hình nhiều.

Là một web sdk (SDK: là một bộ công cụ phát triển).

## Bắt đầu với NextJs

Để bắt đầu với NextJs cần cài đặt và chuẩn bị các thứ như:
- Node.js bản từ 12.22.0 trở lên
- Hđh để lập trình (MacOS, Windows, Linux)
- Code editor (vs code, ...)

### Setup NextJs
Theo như trang chủ yêu cầu, Nextjs sẽ sử dụng `create-next-app` để cài Nextjs project, việc này sẽ diễn ra tự động. Có thể sử dụng **npm** hoặc **yarn** đều được:
```
npx create-next-app@lastest
#or
yarn create next-app
```
Hoặc nếu bạn muốn sử dụng Typescript trong project của mình, có thể thêm flag `--typescript`:
```
npx create-next-app@lastest --typescript
#or
yarn create next-app --typescript
```

NextJs cũng sẽ cài đầy đủ các setup bao gồm: react và react-dom.

Khi cài đặt xong bên trong project sẽ chứa file package.json, những thông tin được chứa bên trong:
```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint"
}
```
- `dev` : khởi chạy Nextjs trong chế độ development
- `build` : Sẽ tạo thư mục build cho project, các file bên trong này sẽ được đưa lên host để chạy app.
- `start` : chạy Nextjs production server.
- `lint` : cài đặt thêm eslint built-in trong project để check lỗi trong khi development.

## NextJs Basic
### Pages

- ### Pre-rendering là gì?

Mặc định, Nextjs sẽ render trước tất cả các page, điều này có nghĩa Nextjs sẽ render các content trong trang HTML trước, thay vì phải đưa cho client và được render bằng Javascript. Việc tải trước này sẽ mang lại hiệu suất tốt hơn và tốt cho SEO.

**Cần tìm hiểu** `hydration`.

- ### Các hình thức Pre-rendering:

Nextjs có 2 hình thức pre-rendering là: **Static generation** và **Server-side rendering**:

- Static generation (nên sử dụng): trang HTML sẽ được tạo tại lúc **build time** (render) và sẽ được sử dụng lại theo từng request.
- Server-side rendering: trang HTML sẽ được tạo sau mỗi lần request được gửi.

Về cách thức tạo static file sẽ gồm 2 loại: có data hoặc không.
- Loại không có data:
```js
function About() {
  return <div>About</div>
}

export default About
```
- Loại có data:
```js
function Blog({ posts }) {
  // Render posts...
}

// This function gets called at build time
export async function getStaticProps() {
  // Call an external API endpoint to get posts
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // By returning { props: { posts } }, the Blog component
  // will receive `posts` as a prop at build time
  return {
    props: {
      posts,
    },
  }
}

export default Blog
```
- ### Khi nào nên sử dụng Static generation:
Nextjs khuyến cáo nên sử dụng static gen (có và không có data) bất cứ khi nào có thể bởi các trang của bạn được built một lần và được serve bởi CDN, điều này sẽ nhanh hơn đối với server render cho 1 trang mỗi lần nhận request.

Các trang có thể sử dụng static generation:
- Marketing pages
- Blog posts và portfolios
- E-commerce product listings (các trang tmdt)
- Help và documentation

- ### Server-side rendering:
Nếu bạn sử dụng SSR thì mỗi lần nhận được request, các trang sẽ được tải lại.

Để sử dụng SSR tạo page, bạn cần `export` một `async` function là `getServerSideProps()`. Function này sẽ được gọi bởi server mỗi request.

```js
function Page({ data }) {
  // Render data...
}

// This gets called on every request
export async function getServerSideProps() {
  // Fetch data from external API
  const res = await fetch(`https://.../data`)
  const data = await res.json()

  // Pass data to the page via props
  return { props: { data } }
}

export default Page
```

### Data fetching

Để fetching data từ Nextjs ta có 3 cách:
- sử dụng `getStaticProps` (static generation): fetch data tại thời điểm build.
- `getStaticPaths` (static generation): render trước các route trước thời điểm build.
- `getServerSideProps` (SSR): fetch data mỗi lần request tới.

- ### getStaticProps()

Nếu bạn xuất `async` function `getStaticProps`, Nextjs sẽ render trước trang đó tại thời điểm build và sử dụng props được trả về từ hàm `getStaticProps`.

```js
export async function getStaticProps(context) {
  return {
    props: {}, // will be passed to the page component as props
  }
}
```
Tham số `context` là một đối tượng chứa những key sau:
- `params` chứa những tham số route phục vụ cho các dynamic route.
- `preview` đặt là true nếu trang trong chế độ xem trước hoặc **undefinded**
- `previewData` chứa data xem trước được set bởi `setPreviewData()`
- `locale` chứa ngôn ngữ đang hoạt động (nếu bạn sử dụng Internaltionalized routing).
- `locales` chứa tất cả các ngôn ngữ hỗ trợ
- `defaultLocale` chứa ngôn ngữ đc đặt mặc định

`getStaticProps()` sẽ trả về một đối tượng với:
- `props` - một đối tượng optional với props được nhận từ page component.
- `revalidate` - (đặt mặc định là **false**)
- `notFound` - một tùy chọn boolean cho phép trang trả về trạng thái 404.

```js
export async function getStaticProps(context) {
  const res = await fetch(`https://.../data`)
  const data = await res.json()

  if (!data) {
    return {
      notFound: true,
    }
  }

  return {
    props: { data }, // will be passed to the page component as props
  }
}
```

**chú ý**: `notFound` sẽ không cần thiết `fallback: flase` vì các paths được return xuất trước từ function `getStaticPaths()`.
- `redirect` - một optional redirect cho phép chuyển hướng trong hoặc ngoài resource. Nó phải phù hợp với `{ destination: string, permanent: boolean }`. 
```js
export async function getStaticProps(context) {
  const res = await fetch(`https://...`)
  const data = await res.json()

  if (!data) {
    return {
      redirect: {
        destination: '/',
        permanent: false,
      },
    }
  }

  return {
    props: { data }, // will be passed to the page component as props
  }
}
```
**Chú ý**: bạn có thể tách import module đặt top-level scope sử dụng `getStaticProps()`. Module sẽ không bundle bên phía client. Điều này có nghĩa bạn có thể viết trực tiếp trong `getStaticProps` (cho cả trường hợp đọc dl từ CSDL).
**Notes**: bạn cũng không nên sử dụng fetch() để gọi API trong `getStaticProps()`. Thay vào đó, hãy import module gọi API để dễ dàng quản lý và sửa.

- ### Khi nào nên sử dụng getStaticProps()

Các trường hợp sử dụng:
- Các dữ liệu hiển thị trang có sẵn tại thời điểm build của người dùng.
- Dữ liệu từ một CMS headless.
- Dữ liệu có thể lưu vào cache (không ở phía user).
- Các trang cần tải trước (cho SEO) và cần tạo nhanh file HTML và JSON.

### Routing

- ### Thành phần Route

Giống như các framework khác, Next.js cũng có hệ thống router được built on.

Khác với React, để tạo 1 route ta cần phải sử dụng component `<Route />` được cung cấp từ `react-router-dom` và phải truyền `path` tham số. Nhưng đối với Next.js, ta chỉ cần tạo file bên trong thư mục `pages`, các file .js sẽ được build thành các route (component -> route).

Ta có 3 loại Route sau:

- **Index routes**: Next.js sẽ tự động nhận các file có tên `index.js` làm route gốc.
```
`pages/index.js` -> '/'
`pages/blog/index.js` -> '/blog'
```
- **Nested routes**: Ta có thể thêm folder bên trong `pages`, và các file được tao bên trong folder đó sẽ được gọi là nested route.
```
`pages/blog/first-blog.js` -> '/blog/first-blog'
```
- **Dynamic routes**: các param của route sẽ được truyền cho file được đặt dấu ngoặc vuông bao ngoài, đây là cách nhận param của route.
```
`pages/blog/[postId].js` -> '/blog/:postId' (vd: '/blog/10')
// postId = 10
`pages/blog/[...all].js` -> '/blog/*' (vd: '/blog/10', '/blog/post/10')
```
- Khi tham số path khớp với một route, nó sẽ gửi một tham số query đến page đó.

Vd: Khi ta truy cập route `post/abc` -> `pages/post/[postId].js` thì đối tượng query được trả về là: `{ "postId": "abc" }`.

- Đối với route `post/abc?foo=bar` thì đối tượng `query` được trả về là:
```js
{ "foo": "bar", "postId": "abc" }
```

**Chú ý**: Trong trường hợp tham số query trong route trùng với tên file, Next.js sẽ ưu tiên lựa chọn query khớp đầu:
```
`post/abc?postId=123` -> { "postId": "abc" }
```

Khi ta sử dụng cú pháp `...` trong dấu ngoặc vuông, route sẽ khớp với tất cả các path chèn thêm vào sau.

**Optional catch all**: Sử dụng `post/[[...slug]].js` sẽ khớp với tất cả các route `/post`, `/post/abc` và `/post/a/b/c`.

- ### Có 4 loại routing phổ biến:
- Pre-defined routes
- Dynamic routes
- Catch all routes
- Optional catch all routes
Mức độ ưu tiên routing:
Pre-defined routes >> Dynamic routes >> Catch all routes `slug.js >> [slug].js >> [...slug].js`.

### Thành phần Link

Đây là thành phần của React, nó được cung cấp thực hiện chuyển đổi các route bên phía client mà không phải reload lại trang

- Để sử dụng ta cần import module Link:
```js
import Link from 'next/link'
```

Link sẽ có props là `href` để điều hướng sang route khác với giá trị `path`.

Có 2 cách để truyền `path` trong Link:
- Truyền thẳng path vào href: 
```js
<Link href="/blog">Post<Link> // -> 'pages/blog.js'
```
- Truyền vào href là 1 đối tượng:
```js
<Link
  href={{
    pathname: '/blog/[slug]',
    query: { slug: post.slug },
  }}
>
  Post
</Link> // -> 'pages/blog/[slug].js'
```

Để truy cập vào đối tượng `router` trong thành phần React, ta có thể sử dụng `useRouter()` hoặc `withRouter()`.

### API routes
Các file API đều được tạo bên trong thư mục `pages/api`, cũng giống như route nó sẽ khớp khi đúng tên file.

Trong các file sẽ là các function xử lý việc trả về response khi nhận request:
```js
export default function handler(req, res) {
  res.status(200).json({ name: 'John Doe' })
}
```

- ### API middleware:
API routes cung cấp các middleware để ta tiện xử lý `request` khi nhận:
- `req.cookies`: đây là đối tượng cookie được gửi từ client, mặc định là `{}`
- `req.query`: là đối tượng chứa query string, mặc định là `{}`
- `req.body`: đối tượng chứa thông tin data gửi từ client.

- ### Response Helper:
Đối tượng `response` bao gồm:
- `res.status(code)`: trạng thái trả về của đối tượng response.
- `res.json(body)`: trả về dữ liệu json
- `res.send(body)`: trả về các kiểu dl: string, object, buffer.
- `res.redirect([status ,] path)`: chuyển hướng tới path khác, optional status mặc định là: 307.

