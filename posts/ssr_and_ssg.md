---
title: 'When to Use Static Generation vs. Server-Side Rendering in Next.js'
date: '2024-01-17'
tags: ["Next.js", "React"]
thumbnailSource: 'https://raw.githubusercontent.com/KennethOnuorah/mdx-blogposts/main/images/thumbnails/ssr_ssg.png'
---

It is recommend to use **Static Generation** (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

You can use Static Generation for many types of pages, including:

- Marketing pages
- Blog posts
- E-commerce product listings
- Help and documentation

You should ask yourself: "Can I pre-render this page **ahead** of a user's request?" If the answer is yes, then you should choose Static Generation.

For instance, let's say you are building a food menu website for your local restaurant where users can navigate to a page on any particular food item on the restaurant's menu. In a case like this SSG would be a preferred option since it is possible to know ahead of time all of the possible food items that can be requested. In Next.js, we can use a built-in function called **generateStaticParams()** to accomplish this. The function returns an array of objects, the objects containing a param for each page that Next.js can pre-render at build time before a user request.

```TypeScript
// src/app/menu/[foodID]/page.tsx

type FoodData = {
  id: string,
  name: string,
  price: number,
}

export async function generateStaticParams() {
  const res = await fetch("https://restaurant.com/api/menu")
  const foodData: FoodData[] = await res.json()
  
  return foodData.map(food => ({
    foodID: food.id
  }))
}

export default function FoodIDPage ({ params } : { params: { foodID: string} }) {
```

On the other hand, Static Generation is **not** a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page shows frequently updated data, and the page content changes on every request.

In that case, you can use **Server-Side Rendering**. It will be slower, but the pre-rendered page will always be up-to-date. Or you can skip pre-rendering and use client-side JavaScript to populate data.