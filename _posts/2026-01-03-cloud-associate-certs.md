---
title: "My certification experience with Azure, GCP, and AWS as a newbie"
date: 2026-01-03
category: [Work Experiences, Certifications]
image: /assets/img/other/cloud-associate-certs/cloud-certs.png
---

On Christmas morning of 2025, I received the **AWS Certified CloudOps Engineer - Associate** certification. This cert is relatively new (it replaced the legacy SysOps Administrator) and it's also the third one I received this year. The full list includes:
1. **Azure Developer Associate** (AZ-204)
2. **GCP Cloud Engineer Associate**
3. **AWS Certified CloudOps Engineer - Associate** (SOA-C03)

Since this was a rather interesting experience, I decided to write a blog post about it, comparing the three associate-level exams.

## From SysOps to CloudOps

At the time, I was a bit surprised by the name change. According to [this blog post by AWS](https://aws.amazon.com/blogs/training-and-certification/exam-update-and-new-name-for-operations-certification/) , they change it *to reflect the evolving nature of cloud operations and a shift in industry terminology*. The key differences are that containers are now in scope and they include more modern services and features. You can view the exam guide to know more about the exam format and topics [here](https://d1.awsstatic.com/onedam/marketing-channels/website/aws/en_US/certification/approved/pdfs/docs-cloudops-associate/AWS-Certified-CloudOps-Engineer-Associate_Exam-Guide.pdf). I will address how relevant this exam is to my work in the next section.


## The Three Associates: A Comparison

Taking all three certifications within a single year wasn’t a pre-planned goal; *rather*, it was a byproduct of my evolving career path this year. It makes more sense to get a professional one after passing an associate one anyway. Initially, I tackled the **Azure** cert during my internship at a startup where it was the primary stack; I pivoted to **GCP** after a friend shared a program offering a 100% voucher for completing specific courses; and finally, I completed the set with **AWS** to align with the main stack used by my current team at NAB Vietnam. The funny thing here is that while NAB is a giant company, it did **NOT** give me any certification voucher, I had to pay for the exam myself unlike the startup which covered the cost for me.

### 1. Azure Developer Associate (AZ-204)

<img src="https://images.credly.com/images/63316b60-f62d-4e51-aacc-c23cb850089c/twitter_thumb_201604_azure-developer-associate-600x600.png" alt="AZ-204" width="150">

#### General Info

- Cost: $83 (with regional cost adjustment applied).
- Duration: 100 minutes
- Number of questions: 50 (although I'm not sure if it's the same for everyone)
- Passing score: 700 or greater (on a scale of 1000)

#### Preparation & Exam Experience

This was my first cloud certification, taken during my internship at a startup. The company was supportive so they covered the exam cost and purchased a Udemy course for me to prepare. I scheduled this exam for March 5th, 2025 and got 2 months to prepare.

Udemy wasn't doing it for me at the time, they taught the basics but that was nowhere near enough for the real exam. Even their practice exams were too easy and not practical. After finding out that this was an open-book exam in which they allow you to search Microsoft docs, I ended up reading only that and taking personal notes as my means of preparation. This turned out to be a massive advantage. During the exam, being able to search the docs allowed me to quickly verify specific SDK details and command parameters that I hadn't fully memorized, significantly speeding up my answers for the more technical questions. Knowing the right keywords to search for was a game-changer and honestly, I think it works that way too in the real world, you encounter a problem and then you search the docs for a solution. 

Compared to the other two exams, this one has more question formats such as drag-and-drop and hot area. Unlike standard multiple-choice, drag-and-drop questions require you to sequence steps or map components, while hot area questions use snippets of code, CLI commands, or UI screenshots where you must select the correct syntax or configuration value from a dropdown menu. For example of hot area questions, they usually require you to complete an `az` CLI command or an XML policy for API Management. The content felt very developer-centric. For instance, you might face a scenario with a web application hosted on **Azure App Service**, requiring you to select the correct environment configuration or scaling strategy to handle traffic spikes.

### 2. GCP Cloud Engineer Associate

<img src="https://images.credly.com/images/08096465-cbfc-4c3e-93e5-93c5aa61f23e/image.png" alt="GCP" width="150">


#### General Info

- Cost: $125
- Duration:  50-60 minutes
- Number of questions: 50 (although I'm not sure if it's the same for everyone)
- Passing score: 700 or greater (on a scale of 1000)

#### Preparation & Exam Experience

I received the exam voucher through the [Get Certified program](https://cloud.google.com/innovators) (no longer available) which required completing Google Skills Qwik Labs courses. My girlfriend introduced me to this program, and after meeting the eligibility requirements, I scheduled the exam for June 10, 2025.

The [exam guide](https://services.google.com/fh/files/misc/associate_cloud_engineer_exam_guide_english.pdf) was my roadmap. I studied for about two weeks, relying mainly on official docs, Qwik Labs, and [Google's sample questions](https://docs.google.com/forms/d/e/1FAIpQLSfexWKtXT2OSFJ-obA4iT3GmzgiOCGvjrT9OfxilWC1yPtmfQ/viewform). My study focused on high-level concepts and understanding *when* to apply specific services.

This strategy was perfect for the actual exam. I found the GCP Associate Cloud Engineer to be the easiest of the three. The questions were scenario-driven, describing a problem and asking for the best service to solve it. Since I had prepared by mapping problems to services, I could spot the answers quickly. Even the IAM questions were manageable with a solid grasp of role hierarchy and security best practices. The questions often presented specific constraints, such as a need for a highly scalable and globally distributed database, asking you to identify the correct **Data** service (like Cloud Spanner) or deciding which **GKE** mode best suits a containerized workload.

### 3. AWS CloudOps Engineer Associate

<img src="https://img-c.udemycdn.com/open-badges/v2/badge-class/1306679240/02182b66-171e-4627-812c-6d3ec94fff921702706523485720870.png" alt="AWS CloudOps" width="150">

#### General Info

- Cost: $150
- Duration: 130 minutes
- Number of questions: 65 with 15 experimental questions not graded.
- Passing score: 720 or greater (on a scale of 1000)

#### Preparation & Exam Experience

I registered on December 2nd and scheduled the exam for December 24th, giving myself less than a month. I felt reasonably confident due to my recent work experience with AWS and the motivation of a 25% discount (plus a free retake). The discount is still available to claim [here](https://pages.awscloud.com/GLOBAL-other-GC-Traincert-Global-Retake-Registration-2025.html) at the time of writing this.

My preparation started with the [official exam guide](https://d1.awsstatic.com/onedam/marketing-channels/website/aws/en_US/certification/approved/pdfs/docs-cloudops-associate/AWS-Certified-CloudOps-Engineer-Associate_Exam-Guide.pdf). Given it was 2025, I enhanced my study with AI, using [Google's NotebookLM](https://notebooklm.google.com/). I uploaded official AWS documentation to the RAG-based tool, which allowed me to generate grounded quizzes, mindmaps, and flashcards without fear of hallucinations. This was highly effective for solidifying core concepts in the first two weeks.

For the final week, I switched to [Neal Davis's practice exams on Udemy](https://www.udemy.com/course/aws-certified-cloudops-engineer-associate-aws-practice-exams/?srsltid=AfmBOopVNmmEi0WVdkS5lRjscDXH4fPPyTBFHk_bELrhTJEnRiYnUxsB&couponCode=KEEPLEARNING), which were on my company's Udemy Business plan. These 65-question exams were very close to the real thing in terms of format, though slightly easier in some areas. I also took the [official practice exam on Skill Builder](https://skillbuilder.aws/learn/P9D6ZEQPZD/official-practice-question-set-aws-certified--cloudops-engineer--associate-soac03--english/GSPTRV2VHG?parentId=D46WYPBKRS). Even though it was official, it has only 20 questions and if you want to do more, you will have to pay them *lol*.

The actual exam was the most difficult of the three; I passed, but just barely. The questions focused heavily on troubleshooting and operational excellence, requiring more than just book knowledge. While the AI tools helped with concepts, the realistic practice exams and my hands-on work interactions were what truly helped me in the end. For instance, one question scenario was that although an IAM role had been granted permission to read and decrypt a KMS key, it received a permission denied error; I instantly knew the answer was to grant access in the resource policy for the KMS key because I encountered this exact issue at work before. Many more questions were like this and I sort of went with my guts because I didn't know the right answer for all of them. It was a rigorous test of troubleshooting skills. It wasn't just about provisioning (but yes, there are questions about that); it was a mix of diagnosing issues across **IAM**, **EC2**, **S3**, and **VPC**, forcing you to dig into *why* something incorrectly configured isn't working.

## Summary Comparison

Looking back at all three, each exam really had its own *vibe*:

- **Azure Developer (AZ-204)** was all about the code—SDKs, APIs, and integrations. The open-book format was a lifesaver, letting me verify syntax on the fly using Microsoft docs. The varied question formats (hot area, drag-and-drop) meant I had to *really* understand how things fit together. **7/10** difficulty; challenging, but the docs access helps if you know what to search for.

- **GCP Cloud Engineer** was, honestly, the easiest. It felt more like *"do you know which tool to use when?"* rather than deep debugging. Scenario-driven questions that are pretty obvious once you've mapped out the services. **5/10** difficulty—straightforward if you've done your homework.

- **AWS CloudOps Associate** was a different beast entirely. Heavy on troubleshooting—figuring out *why* broken setups weren't working across IAM, networking, and monitoring. I barely passed this one, and my work experience saved me more than any study material. **9/10** difficulty—it tests your guts as much as your knowledge.

For prep: **MS Learn docs** for Azure, **Qwik Labs** for GCP, and **Udemy practice exams plus hands-on experience** for AWS.

## Conclusion

Passing all three associate certifications in a single year on my first attempt was a rewarding challenge. Each exam required a different mindset as presented throughout the blog. However, I notice that these things never change across exams or cloud providers: Compute, Storage, Security, Networking, Identity, and Cost Management. If you have a solid understanding of these concepts, studying for these exams will definitely be a breeze.

My next step after this would obviously be to get the **AWS Certified CloudOps Engineer - Professional** certification. I'm also planning on getting the **CKA** as well but this one is both hard and expensive. Might be waiting around for a voucher to take any of these two certs.
