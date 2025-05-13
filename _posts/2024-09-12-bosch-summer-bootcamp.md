---
title: How I spent my summer as an intern at Bosch
date: 2024-09-12 16:36:00 +0700
authors: 
- thu4n
categories: [Work Experiences, Internship]
image: https://res.cloudinary.com/dig6xpboz/image/upload/v1726217517/thu4nAtBosch_qrqxge.jpg
tags: [intern, bosch, infrastructure engineer]
math: true
---

Alright, greeting to anyone reading this, it's been a really long time since I last wrote a blog post in English. So, like the title stated, I spent the last summer joining the internship program at Bosch Global Software Technologies Vietnam (BGSV) and it was a lot of fun! Thereby, I feel that sharing the story here would be nice for me to look back in maybe a few years.

## The Internship Hunt


It was around April of 2024 that I started writing my first ever resume because I wanted to do it in summer. The intern position that I'm initially after is **DevOps**, part of the reason is that I unironically love all the cloud buzzwords and CI/CD thingy. The first and second verion of my resume were *terrible*... since I didn't really know how to write these things. Still, cluelessly decided that it was good enough for submission, I began to look for internship recruitments on the Internet (mainly LinkedIn and Indeed). Unsurprisingly, it was tough at that time with *very few* companies actually hiring DevOps intern. The first one I apply for was the Silver Peacock Solutions company (not their actual name) and of couse, I got dropped at the CV scanning round. They seemed weird anyways as all they hired all years around are interns and not official positions (What's up with that??).

I thought about rewriting my resume at this point and I'm glad I did. I realized that my resume wasn't ticking all the keywords in the job listing. Also, an important factor is timing, applying as early as possible can make the chance of your resume actually getting looked at way higher. Combining the keyword-matching CV and the timing factor, I managed to get an interview and then an offer for DevOps intern. However, the salary for this one is quite low (even for intern standard) so I hold off the offer and continued searching.

Just a few days after, I received an email from the company in this blog's title. They were announcing their short-term internship program called "Bosch Summber Bootcamp 2024". In the list of available positions, what caught my attention was "**Infrastructure Engineer Intern**". The job description said something similar to this: "Bla bla bla Docker container, bla bla bla cloud technologies". Okay, sounds good to me. I immediately rewrote my resume to fit with the JD and then submitted it. About 2 days later, I got the call for interview.

## The Interview

The first round of the interview process is just HR testing my English speaking skill and making sure that I knew what I were applying for. Nothing special to be told here, I passed this rather easily.

Second and final round, the technical interview. I travelled 23km to get to their office and met Mr. TechLead who would be the interviewer. He was pretty chill the whole time and I was the opposite - extremely nervous. All the questions were about what I wrote on my CV. He asked me how I would differentiate Java, Go and C#; a couple questions about Linux commands and OOP pinciples were also asked. He requested me to draw the architecture diagram of one of my projects which used Azure Kubernetes Service and OpenFaaS. Those are the questions that I could remember; I didn't manage to answer all of them perfectly but I did nail the diagram drawing and explaining part. At the end of the interview, he talked a bit more about what his team were working on and why he needed to hire an intern for the Infrastructure Engineer role.

Roughly a week later, I received another call from HR. The call was to announce that I got the offer (Hell yeah). So begin the first internship of my career.

## Wokring at BGSV

### On-boarding

On the on-boarding day, me and a lot of other interns from different posistion were gathered in a seminar room to listen to the on-boarding session presented mainly by HR. I got overloaded with information on that day but to be fair, a handful of them were really important like creating digital signature or how to submit personal documents. At the end of the session, some members of my team (ETA) came to pick me up and showed me where I would be working at in the next few months. 

The entire first week was me preparing required documents and setting up the provided laptop. The company's network banned a lot of stuff so it was fun for me to find out which kind software or website is allowed and which is not. There was an entire guide dedicating to **installing Unikey** in Bosch. My colleagues were all nice and supportive, they taught me how to use the *Cloud Printer* - a super advanced technology that I had never seen before. Jokes aside, I got to do lots of cool stuffs here.

### SDV Runtime

My main role is to build a Docker container which serves as a runtime environment for Software-Defined Vehicle prototypes. Building this took quite some time due to the fact that very few documentations were available at the time. This task helped improve my container knowledge a lot and I got the chance to know the current landscape of open source SDV development. The prototype itself is written in Python but the components interacting with it were written in Rust. The development model for such a prototype is shown in the picture below:

![Velocitas Development Model](https://res.cloudinary.com/dig6xpboz/image/upload/v1730701789/programming_model_hct4rm.png)

This diagram is from the [Velocitas Development Model for SDV Prototypes](https://eclipse-velocitas.github.io/velocitas-docs/docs/concepts/development_model/) and of course, it is open source so I can freely talk about this. The Docker image that I was tasked to build need to contain 95% of the things in the diagram. A brief break down is that there is a Python Software Development Kit (SDK) that allows developers to interact with vehicle physical components via Python code. These code will then often use Vehicle APIs (VSS) and there will be a broker (Kuksa Databroker) to receive these  API calls and distribute them accordingly. Networking between these services is mainly via gRPC, the MQTT part is only for cloud connection. More details are included in the link I provided, this is just the gist of it.

Aside from the fact that the container should be able to run, e.g. operating with all services in the Velocitas Development Model , I think the main concern when building a Docker image is the image size. In my situation, not only does it run on a small Azure VM, it also has to run on a Raspberry Pi with limited disk storage. Obviously with the Pi comes into play, image architecture is taken into account as well. To sum up, the container requirements were:

1. *It can run*.
2. It is small (below 300MB).
3. It support AMD64 and ARM64.

For requirement #1, I can't talk much due to the main code being private. The SDK is just one `pip install`, the whole VSS system is confusing at first because some versions don't really work with the others. The broker's code was Rust so compiling it with Docker build was way too long. For some reasons, cargo - a tool which mangages and compiles Rust crates couldn't be cached normally by the Docker build toolkit. I initially resolved this by just writing a bash script to comiple the binary beforehand and copy it into the Dockerfile later. Add in a little magic, I got it up and running in my first 2 weeks at Bosch.

I spent the following week fufilling requirement #2 and #3. For a smaller image, usually you start with a small base like Alpine Linux. However, some libraries that I need couldn't work with an Alpine-based image due to `musl` being its C standard library and not the popular `glibc` that we all know and love. Thereby, I went with Ubuntu 22.04 which was only about 80MB in size and it worked really well with all the libs and crates. I then apply the **multi-stage Docker build** technique which basically means that you do all the heavy work of compiling in the first stage and then copy all the artifacts into the final stage. This help reduce image size because every `RUN` command in Dockerfile is a layer in  your image so you want as few of them as possible. Another trick is to combine multiple shell commands into one `RUN` command using `&&` but this make your Dockerfile a bit harder to read so I don't really recommend it. For a multi-architecture build, I used the Docker Buildkit (`docker buildx`) with the [Moby Builder](https://github.com/moby/buildkit) as the driver. After a few steps of setup,  I could simply use `docker buildx build --platform linux/amd64,linux/arm64`.

The next step is to automate the container building process with GitHub Actions. I wrote a fairly simple CI workflow which includes a matrix strategy to run the broker compiling script for AMD64 and ARM64, a few steps here and there to set up Docker and cargo cache and then just build for both architecture and push the image with the commit SHA ID (shorthand version) as the image tag. Deployment is just your usual container image pulling with the DockerHub's webhook.

After that, it's all about perfomance tweaking. The Docker image is publicly available at: https://hub.docker.com/r/boschvn/sdv-runtime. To this day, I'm still proud of it because it's not perfect but it's good enough for my team!

### Rust compiler server

Thanks to this task, I realize how much I hate Rust. Not much details I can talk about here but essentially, I have to build a server that takes in Rust code, compile it and return the binary. The core logic sounds simple but it's the application build-caching part that was a pain in the ass, especially for someone with no Rust experience! Not only that, Docker build itself is so painfully slow and hard to cache as well. I used [cargo-chef](https://github.com/LukeMathWalker/cargo-chef) for a faster Docker build but even this came with its own limitation (see the README of the project).

### Soft-skill training

A perk that I got for joining the Summber Bootcamp is that the HR department prepared a training program for us to participate every week. I learned about critical thinking, presentation giving, time management and a lot more. Often, we would be split into groups and worked together to either debate or present about a certain topic. It was fun overall and some advices were really solid. Unfortunately, I couldn't join the last day of the program for celebration since I had to return to my uni for my internship report at the time. Overall, I think it was a great program, the HR team did a really good job setting this up.

## End of internship and conclusion

*This is written as of May 15th, 2025 (super delay, lol).*

Originally, it was a 3-month internship but I got it extended to 6 months. After it ended, no conversion, unfortunately. HR told me there was no head-count at the time so there's that.

I think for my first IT job ever, this went pretty well. Almost all tasks given were completed, I took ownership in my work and I didn't cause the team any trouble. Seeing how slow and complicated all the process in a big company going is certainly an experience. In conclusion, I enjoyed my stay at Bosch.