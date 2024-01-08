- To set rule during manual pipeline trigger:
```yaml
rules:
   - if $CI_PIPELINE_SOURCE == "web":
     when: never
```
- To set rule during merge request event:
```yaml
rules:
   - if $CI_PIPELINE_SOURCE == "merge_request_event":
     when: never
```
- Gitab rules need a default case

- Parent job rules get overridden by child job rules.  
https://stackoverflow.com/questions/72545625/gitlab-ci-rules-not-working-with-extends-and-individual-rules
https://stackoverflow.com/questions/68227282/gitlab-ci-define-rules-globally-and-overwrite-them-locally-in-a-stage
  
- what is CI_COMMIT_REF_NAME? -->  https://stackoverflow.com/questions/66526199/gitlab-ci-rules-not-triggering-job
