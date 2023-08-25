# DevOps Assignment3 Github Actions

## Sample html file hosting on AWS EC2 with a CI/CD Setup Using GitHub Actions

## workflow 
```yaml
name: Push-to-EC2

# Trigger deployment only on push to main branch
on:
  push:
    branches:
      - main

jobs:
  deploy:
    name: Deploy to EC2 on master branch push
    runs-on: ubuntu-latest

    steps:
      - name: Checkout the files
        uses: actions/checkout@v3

      - name: Deploy to Server 1
        uses: easingthemes/ssh-deploy@main
        env:
          SSH_PRIVATE_KEY: ${{ secrets.EC2_SSH_KEY }}
          REMOTE_HOST: ${{ secrets.HOST_DNS }}
          REMOTE_USER: ${{ secrets.USERNAME }}
          TARGET: ${{ secrets.TARGET_DIR }}

      - name: Executing remote ssh commands using ssh key
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST_DNS }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            sudo apt-get -y update
            sudo apt-get install -y apache2
            sudo systemctl start apache2
            sudo systemctl enable apache2
            cd home
            sudo mv * /var/www/html


```
## check on Remore server that all files have been copied or not 
```
ubuntu@ip-172-31-0-135:/var/www/html$ pwd
/var/www/html
ubuntu@ip-172-31-0-135:/var/www/html$ ls -l
total 12
-rw-r--r-- 1 ubuntu ubuntu  144 Aug 25 07:15 README.md
-rw-r--r-- 1 ubuntu ubuntu 1747 Aug 25 07:15 index.html
-rw-r--r-- 1 ubuntu ubuntu  583 Aug 25 07:15 style.css
ubuntu@ip-172-31-0-135:/var/www/html$ 

```
## using curl command to check if html page is displayed or not
```
curl http://localhost:80
```
**Output**
```html
<!DOCTYPE html>
<html>
<head>
        <title>Resume</title>
        <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
        <div class="container">
                <header>
                        <h1>DiceCamp Assignmet 3</h1>
                        <p>Sample Resume for Web Developer</p>
                </header>
                <section>
                        <h2>Summary</h2>
                        <p>A highly motivated web developer with 5+ years of experience creating dynamic and responsive web applications. Strong skills in HTML, CSS, and JavaScript, with a passion for creating visually appealing and user-friendly interfaces. Committed to staying up-to-date with the latest trends and technologies in the industry.</p>
                </section>
                <section>
                        <h2>Skills</h2>
                        <ul>
                                <li>HTML</li>
                                <li>CSS</li>
                                <li>JavaScript</li>
                                <li>jQuery</li>
                                <li>Bootstrap</li>
                                <li>React</li>
                        </ul>
                </section>
                <section>
                        <h2>Experience</h2>
                        <h3>Web Developer, XYZ Company</h3>
                        <p>Developed and maintained multiple company websites using HTML, CSS, and JavaScript. Collaborated with designers and project managers to create visually appealing and user-friendly interfaces. Conducted regular code reviews to ensure high-quality and efficient code. </p>
                        <h3>Web Developer, ABC Agency</h3>
                        <p>Designed and implemented custom WordPress themes and plugins. Worked closely with clients to gather requirements and provide technical support. Conducted regular testing and debugging to ensure optimal performance.</p>
                </section>
                <section>
                        <h2>Education</h2>
                        <h3>Bachelor of Science in Computer Science</h3>
                        <p>University of ABC, Graduated May 20XX</p>
                </section>
                <section id="projects">
                        <h2>Projects</h2>
                        <p>sample project hosting on EC2 using CI/CD Pipelines and github actions </p>
                </section>
        </div>
</body>
</html>
```
## If instance is not stopped then can be accessed via 
```http://ec2-43-204-101-29.ap-south-1.compute.amazonaws.com/```
