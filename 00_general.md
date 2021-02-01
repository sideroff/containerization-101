# CI, CD

## Continious integration

Basically make sure that each new addition to the code is:

1. buildable
2. testable ( and passes tests )

This way no new broken code gets accepted in the repo.

## Continious delivery

Automatically deploy code that has passed CI steps to your environments.

## Note

The most extreme example of these practises would have you build every single commit and deploy every single change up to production automatically. Of course, even with the most vigorous testing this might be something you don't want, so a more common case is having CI get triggered on PR, and CD to automatically deploy up to your testing environment, and have manually triggered deploys for test & prod.
