# Lab 09: Knowledge check

**Answer each question below it, in your own words, in one to three sentences.** Replace the `...` below **Your answer:** with your text.

## 1. Credentials in the image?

Assume you want to share your image with others and make it publicly available on `ghcr.io`, so that anyone can pull it and run the analysis. A teammate suggests adding `COPY .aws /home/app/.aws` to the `Dockerfile`, so that nobody has to type `-v ~/.aws:/home/app/.aws:ro` any more. (This is a thought experiment: keep your own package private.)

**Your answer:** Give two reasons why this is a bad idea. Think of who could then read your credentials, and of what happens when your Learner Lab session ends.

This is a bad idea because my Learner Lab credentials expire when the session ends, so they would eventually stop working. Also, anyone who downloads the image could access my AWS credentials.

## 2. Where did the Atlas password come from?

In Step 4, you ran the analysis once with `-v ~/.aws:/home/app/.aws:ro` only, without the three `-e` options. The container still wrote to your Atlas cluster.

*Hint:* Read `get_atlas_settings()` in `src/book_analysis/analyze.py`.

**Your answer:** Explain, step by step, where the container found the Atlas settings in that run.

The container accessed the AWS credentials because the `-v` command mounted the local `.aws` folder inside the container at `/home/app/.aws`. The `get_atlas_settings()` function likely used those credentials to retrieve the MongoDB Atlas settings, allowing the analysis to connect to the cluster without the three `-e` options.

## 3. Same tag, different image

Run `docker images book-analysis`, and note the image ID of `book-analysis:0.1.0`. Then change the `description` in the `LABEL` of your `Dockerfile`, build again with the same command, `docker build -t book-analysis:0.1.0 .`, and run `docker images book-analysis` again.

**Your answer:** Paste the image ID from before and after the rebuild. Why is it a problem for someone who uses your image that `0.1.0` now names a different image, and what should you have done instead?

Image ID BEFORE: 59924d780a85
Image ID AFTER: 22463369fc4e

This is a problem because the same version tag now points to a different image, which can confuse people using it. I should have given the updated image a new version tag, like 0.1.1.
