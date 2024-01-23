```
stages:
  - build
  - test
  - deploy

before_script:
  - echo "Before script section"
  - echo "For example you might run an update here or install a build dependency"
  - echo "Or perhaps you might print out some debugging details"

after_script:
  - echo "After script section"
  - echo "For example you might do some cleanup here"

build1:
  stage: build
  script:
    - echo "Do your build here"
    - mkdir -p tests
    - echo one >> tests/test1.txt
    - echo two >> tests/test2.txt
  artifacts:
    paths:
      - tests
    exclude:
      - tests/test2.txt
    expire_in: 30 days

test1:
  stage: test
  script:
    - echo "Do a test here"
    - grep one tests/test1.txt

test2:
  stage: test
  script:
    - echo "Do another parallel test here"
    - grep two tests/test2.txt

deploy1:
  stage: deploy
  script:
    - echo "Do your deploy here"
  environment: production

pages:
  stage: deploy
  script:
    - mkdir -p public
    - cp tests/test1.txt public/index.html
  artifacts:
    paths:
      - public
  only:
    - main
```