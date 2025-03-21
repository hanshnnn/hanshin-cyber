---
authors:
  - name: Han Shin
    link: https://github.com/hanshnnn
    image: https://avatars.githubusercontent.com/u/65099129?v=4
date: 2025-03-21
linktitle: Server Side Security Notes
title: Server Side Security Notes
tags:
  - Study
  - Servers
  - Networks
---

Hello, this is my first reseach-study blog post ;p Currently solving a server side security wargame (doing the writeup soon)

## Robots Exclusion Protocol

{{< callout type="info" >}}
  Standard used by websites to indicate which portions of website are allowed to visit
{{< /callout >}}

- for web crawlers and robots

- `robots.txt` can be found [https://www.example.com/robots.txt](https://www.example.com/robots.txt)

- the robots will try to fetch this file and read the instruction, if the file doesn't exist means → no restrictions

 

### AI restrictions

web operators start to ban robots collecting data to train AI models. In 2023, Originality.AI found that 306 of the thousand most-visited websites blocked [OpenAI](https://en.wikipedia.org/wiki/OpenAI)'s GPTBot in their robots.txt file.

In 2023, blog host [Medium](https://en.wikipedia.org/wiki/Medium_(website)) announced it would deny access to all artificial intelligence web crawlers as "AI companies have leached value from writers in order to spam Internet readers". - 
https://www.theverge.com/24067997/robots-txt-ai-text-file-web-crawlers-spiders

### Format Example

1. Allow all content
    
    ```jsx
    User-agent: *
    Disallow:
    ```
    
    or
    
    ```jsx
    User-agent: *
    Allow: /
    ```
    
    or even a missing robots.txt file
    
2. All robots to stay out
    
    ```jsx
    User-agent: *
    Disallow: /
    ```
    
3. Not to enter certain directories
    
    ```jsx
    User-agent: *
    Disallow: /cgi-bin/
    Disallow: /tmp/
    Disallow: /junk/
    ```
    
4. Not to enter one file
    
    ```jsx
    User-agent: *
    Disallow: /directory/file.html
    ```
    
5. **Specific** robot “example: BadBot” to stay out
    
    ```jsx
    User-agent: BadBot
    Disallow: /
    ```
    

Combined example

```jsx
User-agent: googlebot        # all Google services
Disallow: /private/          # disallow this directory

User-agent: googlebot-news   # only the news service
Disallow: /                  # disallow everything

User-agent: *                # any robot
Disallow: /something/        # disallow this directory
```

From the above example, all Google services are not allowed to access /private/ directory, news services are not allowed to access the whole site. Meanwhile any robots are not allowed to access /something/

## Command injection

{{< callout type="info" >}}
  command injection is a type of attach, the goal is to execute arbitrary commands on host operating system
{{< /callout >}}


Happens when:  
- application passes unsafe user-supplied data like forms, cookies
- insufficient input validation

Example:

```php
<?php
    $ip = $_GET['ip'];
    system("ping -c 4 " . $ip);
?>
```

If the attacker inputs `8.8.8.8; cat /etc/passwd` , then it will pass to server hence executing  `cat /etc/passwd` 

### Tools

- [commixproject/commix](https://github.com/commixproject/commix) - Automated All-in-One OS command injection and exploitation tool
- [projectdiscovery/interactsh](https://github.com/projectdiscovery/interactsh) - An OOB interaction gathering server and client librarys