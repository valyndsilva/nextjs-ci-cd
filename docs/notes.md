npx create-next-app nextjs-ci-cd
mkdir .github
mkdir .github/workflows
touch .github/workflows/simple.yml

In .github/workflows/simple.yml:
```
name: simple-pipeline

on:
    push:
        branches: [main]
        
jobs: 
        lintTest:
            name: Lint
            runs-on: ubuntu-latest
            steps:
                - name: Clone Repository
                  uses: actions/checkout@v2
                - name: Install Dependencies
                  run: |
                    echo "Running Install Dependencies..."
                    npm install
                - name: Run Linting
                  run: |
                    echo "Running Linting tests..."
                    npm run lint
                  env:
                    CI: true

```

In Terminal:
```
git add .
git commit -m "initial commit"
git remote add origin https://github.com/valyndsilva/nextjs-ci-cd.git
git push -u origin main
```

Now check Github Repo > Actions > workflow

Update simple.yml:
```
name: simple-pipeline

on:
 push:
    branches: [main]
        
jobs: 
    lintTest:
        name: Lint
        runs-on: ubuntu-latest
        steps:
            - name: Clone Repository
              uses: actions/checkout@v2
            - name: Install Dependencies
              run: |
                echo "Running Install Dependencies..."
                npm install
            - name: Run Linting
              run: |
                echo "Running Linting tests..."
                npm run lint
              env:
                CI: true
    securityCheck:
        name: Security Check
        runs-on: ubuntu-latest
        steps:
         
            - name: Check for Security
              run: |
                echo "Running Security Checks 1..."
                sleep 5s;
                echo "Running Security Checks 2..."
                sleep 5s;
                echo "Running Security Checks 3..."
                sleep 5s;
                echo "Running Security Checks 4..."
                sleep 5s;

```


```
git add .
git commit -m "added security checks"
git push
```

Update simple.yml:
```
name: simple-pipeline

on:
 push:
    branches: [main]
        
jobs: 
    lintTest:
        name: Lint
        runs-on: ubuntu-latest
        steps:
            - name: Clone Repository
              uses: actions/checkout@v2
            - name: Install Dependencies
              run: |
                echo "Running Install Dependencies..."
                npm install
            - name: Run Linting
              run: |
                echo "Running Linting tests..."
                npm run lint
              env:
                CI: true
    securityCheck:
        name: Security Check
        runs-on: ubuntu-latest
        steps:
         
            - name: Check for Security
              run: |
                echo "Running Security Checks 1..."
                sleep 5s;
                echo "Running Security Checks 2..."
                sleep 5s;
                echo "Running Security Checks 3..."
                sleep 5s;
                echo "Running Security Checks 4..."
                sleep 5s;
    deploy:
        name: Deployment
        runs-on: ubuntu-latest
        needs: [lintTest, securityCheck]
        steps:
            - name: Install Dependencies
              run: |
                echo "Deployment in progress..."
               
```


```
git add .
git commit -m "added deployment"
git push
```

Updates eslintrc.json:
```
{
  "extends": "next/core-web-vitals",
  "rules":{
    "no-unusued-vars":[
      "eror",
      {"vars": "all", "args":"after-used", "ignoreRestSiblings":false}
    ]
  }
}

```

Add some error in a file Ex: app/page.tsx
```
import { Router } from 'next/router'  // unsued
```

```
git add .
git commit -m "fail test"
git push
```

If any of the jobs fail it won't proceed with deployment.