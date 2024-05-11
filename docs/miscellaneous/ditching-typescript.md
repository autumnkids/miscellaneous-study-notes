# Big Projects Ditching TypeScript, and Why?

This is an interesting point of view, and I would like to put it down somewhere for future references, if need to.

It's a video from Youtube that caught my eyes: [Big projects are ditching TypeScript… why?](https://youtu.be/5ChkQKUzDCs?si=CulgAQQ9FTK-ZSxG).

I personally have got the chance to learn TypeScript and use it in projects. But the first time I heard about the idea, I personally liked it.

I come from a strongly typed programming language background, and would think that any tool that can help me identify a potential type error during dev or compile time would be helpful, such that such errors can be caught before runtime.

While TypeScript does the job, the main points that were mentioned in the video were that:

* It pollutes the code with type gymnastics.
* It requires explicit compile step.

I kinda agreed with the first point when I firstly heard about how to define types with TypeScript. But I was also viewing that as a tradeoff to introduce type safety in Javascript.

I don't have a lot experience and data to tell how much of a problem the second point can bring in. But it's something that can be benchmarked.

An alternative that was mentioned in the video is `JSDoc`.

I think what is worth noting is that ECMAScript is having a [proposal](https://github.com/tc39/proposal-type-annotations) that aims to enable developers to add type annotations to the Javascript code. When it becomes a standard, it'll be interesting to compare the option with TypeScript and JSDoc, and see what areas each of those excels at.
