---
layout: post
title:  "Pass Multiple Secrets into a Dockerfile"
categories: docker secrets
---
![oooh girl!](/assets/images/secrets_image.jpeg)

Say you're developing a gem that depends on another gem hosted in Github Packages. You need to install this private gem during the image build phase of Docker:
```
gem install private_gem:1.0.0 \
--source https://<USERNAME>:<TOKEN>@rubygems.pkg.github.com/<OWNER>
```
But putting this in the Dockerfile as-is bakes the secrets straight into the source code. That's Super Bad. So how do you securely pass these values into the build process?

## Single Secrets
The accepted practice has been with Docker BuildKit since v18.09. If you google for this you'll come across numerous blog posts copying the same tutorial from Docker's documentation.

Look familiar?
```docker
RUN --mount=type=secret,id=mysecret cat /run/secrets/mysecret
```
Then building the image while passing in a secret file:
```docker
docker build --no-cache \
             --progress=plain \
             --secret id=mysecret,src=mysecret.txt \
             .
```
That works for 1 secret, but what about our multiple secrets use-case with username and password?

## Multiple Secrets as Individual Files
You could create a secret file for each variable and pass each one in an extra `--secret` param.

```
# username_file.txt
JohnSmith
```
```
# password_file.txt
mysupersecretpassword
```
Mount, then reference each secret in the RUN statement:
```docker
RUN --mount=type=secret,id=username \
  	--mount=type=secret,id=password \
    gem install private_gem:1.0.0 \
    --source https://$(cat /run/secrets/username)\
:$(cat /run/secrets/password)@rubygems.pkg.github.com/<OWNER>
```
And run the build command:
```
docker build \
       --secret id=username,src=username_file.txt \
       --secret id=password,src=password_file.txt \
       .
```
This works, but is also a Terrible Idea. For a real world application I would prefer to not have dozens of one-liner secret files cluttering up my filespace.

## Multiple Secrets in a Single File
What we want is a KEY=VALUE file.

Because these docs and blog posts copy one another, it's easy to miss that what is actually happening is the entire secret file is being mounted.

If it is a file, then we can grep out the secrets in the mounted secret file during execution of the run commands. There are [multiple ways to do this](https://stackoverflow.com/a/30776327/4029445), I like to use the `sed` option. The `source` option has too many edge cases like being unable to handle whitespace in the values.

I wish there was an easier way to do this. Perhaps as untraceable environment variables, but that doesn't exist as far as I know? Passing the secrets as normal `ENV` variables would bake them into the image as well, so that wouldn't work either.

Anyway, let's test this out.

Create a secrets file with a couple of harmless secrets as KEY=VALUE pairs.
```
# secrets.txt
HARMLESS_SECRET=Hello World
HARMFUL_SECRET=;)
```
Start with something benign as grabbing and printing to output. The secret will only be present during execution of the RUN command, so shouldn't be exposed upon publication.
```docker
RUN --mount=type=secret,id=secret_file \
    echo $(sed -n 's/^HARMFUL_SECRET=//p' /run/secrets/secret_file)
```
Then run the build command:
```
docker build --progress=plain --no-cache \
             --secret id=secret_file,src=secrets.txt .

...
#7 [2/2] RUN --mount=type=secret,id=secret_file     echo $(sed -n 's/^HARMFUL_SECRET=//p' /run/secrets/secret_file)
#7 sha256:e796506596040f592e18cd424bd63cf6540d3fd440cb87791e3aa5eb06589cac
#7 0.259 ;)
#7 DONE 0.3s
...
```

Now let's take what we learned and apply it to a real world application. Here is my gem install line:
```docker
RUN --mount=type=secret,id=secret_file \
    gem install private_gem:1.0.0 \
    --source https://$(sed -n 's/^GH_USERNAME=//p' /run/secrets/secret_file)\
:$(sed -n 's/^GH_TOKEN=//p' /run/secrets/secret_file)@rubygems.pkg.github.com/<OWNER>
```
My build command:
```
docker build --secret id=secret_file,src=secrets.txt .
```
That's how you pass multiple secrets into a Dockerfile during the build phase. Hope you found it helpful!
