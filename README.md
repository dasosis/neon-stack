# Neon Stack

A tiny single-page arcade game: stack sliding blocks as high as you can. Overhangs get sliced off; perfect drops build combos and win back width.

**Play:** https://d1kl30gpj8omia.cloudfront.net

## Controls

Tap, click, or press <kbd>Space</kbd> to drop the block. Works on desktop and mobile.

## Run locally

It's one self-contained file with no build step or dependencies. Open `index.html` in a browser.

## Hosting on AWS

Served from a private S3 bucket through CloudFront using Origin Access Control (OAC):

- S3 bucket with Block Public Access enabled. Only the CloudFront distribution can read it, via a bucket policy scoped to the distribution ARN.
- CloudFront redirects HTTP to HTTPS and uses the managed `CachingOptimized` cache policy and the `SecurityHeadersPolicy` response headers policy.

### Deploying an update

```sh
aws s3 cp index.html s3://<BUCKET_NAME>/index.html \
  --content-type "text/html; charset=utf-8" --cache-control "max-age=300"

# optional: make the change visible immediately instead of within ~5 minutes
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths "/*"
```
