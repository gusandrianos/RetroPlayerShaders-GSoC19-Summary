# OpenGL back-end for RetroPlayer's shaders - Google Summer of Code 2019

## Introduction
This is the first time I've had the chance of participating in GSoC and I hope you have as much fun trying my project, as I had making it.

This project aims to bring the functionality [this](https://velocityra.github.io/gsoc-2017) GSoC 2017 project introduced to RetroPlayer to every platform Kodi supports, through OpenGL and OpenGL-ES back-ends.

You can see it in action in this video.
[![YouTube Demo](https://github.com/KostasAndrianos/RetroPlayerShaders-GSoC19-Summary/blob/master/resources/preview.png?raw=true)](https://www.youtube.com/watch?v=2Y_Eo7ZL0Ks&feature=youtu.be)

## Challenges
To give you a bit of context, this is the first time I'm working with OpenGL and also the first time I'm working on such a significant project with such a huge codebase.

In no particular order, here are some challenges I faced both with the project and the tools I used.
- Working with OpenGL required a good amount of studying and only after writing and, more importantly, debugging a lot of code, I really got a good understanding of it.
- I had to unlearn some bad practices I inevitably adopted through lots of university courses and small projects, that didn't really have to be modular or maintainable.
- Most importantly, I had to use and modify existing code, that I didn't write and I'd never seen before, which required patience, a lot of navigating around a foreign codebase and the ability to get in the shoes of whoever wrote it.

Regarding the project
- There was already some level of abstraction which both restricted and helped me. Some interfaces were written with DirectX in mind, meaning I had to implement some empty methods, just to satisfy them, and I also had to pass unused parameters to some methods I used.
- The availability of shader presets between the cg and GLSL repos wasn't the same, so I had to throw away some presets from the original list and pass the new paths for others. This changed the experience one would have by offering different presets on different platforms.
- Setting up the development environment had a steep learning curve and I spent a fair bit of time compiling and configuring, both on Windows and Linux.
- Rendering multi-pass presets was tricky and a lot of them do not currently work.

## Work
We initially talked about going with [slang](https://github.com/shader-slang/slang) but we later steered away from it and decided it was better to stick to the original plan.

I spent the better part of the first month studying code, building my environment and becoming familiar with the structure and the functionality. Some days were spent reading up on slang and I also had some university obligations, so I picked up coding near the very end of June.

On the second month, I started implementing major parts of the project but I was coding "in the dark" as I didn't have all the pieces in place, to visualize my changes. Luckily, I had already seen what the desired result was and having the interfaces and existing code as a reference helped me get together a well-structured solution, that would set me up for an easier debugging period.

On the last month, I finally connected my code to RetroPlayer and started debugging. This month has been the most productive part of GSoC for me, as the pressure of the deadline gave me extra motivation to work hard and reach the project goal. I started working towards getting my code to display something. After that happened, I got most single-pass presets to work and gradually moved to dual-pass and multi-pass presets, however, most of those do not currently work.

You can find all the changes and instructions to use my code in this [work-in-progress pull request](https://github.com/garbear/xbmc/pull/114).
The last commit that should be considered inside the GSoC 2019 period is [this](https://github.com/KostasAndrianos/xbmc/commit/e6e3dab0342f4f9e0cd567e3ff03e6d9e74d2266) one. As of right now, my code isn't merged.

## Bugs
### Minor
- The cursor tends to cause flickering in some cases, however, it appears randomly and therefore it's not easily reproducible.
### Major
- Under the menu `Settings->Video Filter`, there is a list with the available shader presets. Normally, this list would display real-time previews of each preset on the thumbnails and when selected, on the screen. As of right now, none of these work. Here's an image to illustrate the issue.
![Black previews bug](https://github.com/KostasAndrianos/RetroPlayerShaders-GSoC19-Summary/blob/master/resources/black_previews_bug.png?raw=true)
- Lookup textures (LUTs) aren't supported yet, even though I already implemented almost all the logic, I couldn't get them to work.
- Following the previous issue, any presets that heavily rely on LUTs, do not display what they're supposed to or, most of the time, anything at all. I used 2048 with the preset `reshade/halftone-print.glslp` as it is a core game on both Kodi and RetroArch and, therefore, ideal for a side-to-side comparison, to better illustrate the issue. The second one is correct, the first one isn't.
![Buggy halftone](https://github.com/KostasAndrianos/RetroPlayerShaders-GSoC19-Summary/blob/master/resources/halftone-print-buggy.png?raw=true)
![Correct halftone](https://github.com/KostasAndrianos/RetroPlayerShaders-GSoC19-Summary/blob/master/resources/halftone-print-working.png?raw=true)
- Custom borders don't work as they're loaded through LUTs.
- The majority of multi-pass presets (3+ passes) don't work but I suspect that many of those will, after I successfully implement the LUT support. Some do work perfectly fine, so this is probably a good indicator that the rendering chain logic is solid.

## Future Work
### Important
- First of all, support for LUTs must be added as it will remove a point of failure for multi-pass presets.
- The code should be able to render most of the multi-pass presets along with custom borders.
- Ultimately, this project aims to support all platforms Kodi does, so OpenGL-ES is the final step towards hitting that target.
### Nice-to-have
- There should be a way to change the uniform parameters some shaders have. As of right now, the parameters are set to their default values.
- We aim for a user-friendly experience with custom descriptions for a limited selection of presets but we could add a way for users to choose any preset they like.

## Acknowledgements
- I'd like to thank my mentor, Garrett Brown (garbear) for answering all of my(sometimes stupid) questions, helping me solve some technical issues and generally for being such an awesome, positive and fun person.
- I'd like to thank my friend Nick Renieris (velocity) for the countless times he gave solutions to my problems and for explaining certain parts of the code he wrote for the project, in GSoC 2017. He was the one who suggested I'd take this challenge and I'm really happy I did.
- Finally, I'd like to thank lrusak, fritsch, yol, keith, spiff, razze, natethomas and the rest of the Kodi team for all the help they gave me, the opportunity to work on Kodi for this year's GSoC and for being such a welcoming and fun group of people!

## Closing thoughts
Looking back at everything that happened during GSoC and the months before that, I can say it has been a blast.
We started talking about the project with Nick(velocity) back in Febuary and if you told me I would be here today, writing this summary, I'd really laugh at you back then. It required a lot of effort, hard work and dedication and I'm glad it was a success.

I believe it was a very productive summer and I've really embraced the philosophy of Google Summer of Code as I've learned a **ton** in such a short period of time. I really liked the project and it's safe to say I will continue to contribute to it, after GSoC ends. 
