# Lab 09: Knowledge check

**Answer each question below it, in your own words, in one to three sentences.** Replace the `...` below **Your answer:** with your text.

## 1. Credentials in the image?

Assume you want to share your image with others and make it publicly available on `ghcr.io`, so that anyone can pull it and run the analysis. A teammate suggests adding `COPY .aws /home/app/.aws` to the `Dockerfile`, so that nobody has to type `-v ~/.aws:/home/app/.aws:ro` any more. (This is a thought experiment: keep your own package private.)

**Your answer:** Give two reasons why this is a bad idea. Think of who could then read your credentials, and of what happens when your Learner Lab session ends.
this is a bad idea because aadding the copy statement would add my credentials for my "account" to the container. This means poeple even without credentials could have access to the AWS services, which using my credentials has access to some private s3 buckets. We also get cerdentials refreshed, and this would not account for that, meaning the container would use meaningless credentials.

...


## 2. Where did the Atlas password come from?

In Step 4, you ran the analysis once with `-v ~/.aws:/home/app/.aws:ro` only, without the three `-e` options. The container still wrote to your Atlas cluster.

*Hint:* Read `get_atlas_settings()` in `src/book_analysis/analyze.py`.

**Your answer:** Explain, step by step, where the container found the Atlas settings in that run.

It first checked the environment variables to see if any matching "MONGODB_ATLAS_URL" were created, and if so, it reads from those environment variables, and returns the url, user, and pwd in the same format the SSM parameter call would return.

if not, which without the three -e options is what happen becasue it will not reach the container, it will use the SSM in AWS to call those parameters. It used the credentials we had set in the home directory to access this data.



## 3. Same tag, different image

Run `docker images book-analysis`, and note the image ID of `book-analysis:0.1.0`. Then change the `description` in the `LABEL` of your `Dockerfile`, build again with the same command, `docker build -t book-analysis:0.1.0 .`, and run `docker images book-analysis` again.

**Your answer:** Paste the image ID from before and after the rebuild. Why is it a problem for someone who uses your image that `0.1.0` now names a different image, and what should you have done instead?
Old:
book-analysis:0.1.0   94cdc5a6dedf
New:
book-analysis:0.1.0   99be88165af4

I shoudl have changed the versiuon because although a different ID, it is easier to understand that it is a different version when you change the actual name. Someone also could have pulled from the old ID, and now we have both under the same name, so we both can say we are runnign the same version because we have the same name, but we might not be running the same thing.

