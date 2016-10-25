<!--
https://api.github.com/users/dave45678/repos
-->
###Use JavaScript to display your GitHub projects
####and host them for free on GitHub pages


**Prerequisite**: you should have multiple GitHub repositories already. If not, go back and create some. If you use linkedIn or have a blog you wish to link from this site then you can. I'll also show you how to redirect a custom domain to this site as well. You can use your existing domain or register a new domain. 

GitHub Pages is a free offering from GitHub.com. Pages allows you to host pages. The pages can be for a particular project. Or they can be for something else. Today we're going to look at something else - a resume. GitHub Pages sites are publicly available on the internet. 

**Technologies**: We will be creating this solution with GitHub, Prose, JavaScript, HTML, BootStrap, Gravatar. You will not need to install anything on your computer. Everything we use in this tutorial is free. You can also register a domain with  hover.com or another provider. Registering the domain is not free. You do not need to register a domain for this tutorial.  

####Organize your existing GitHub repositories
This page will list repositories that have a description. If you have a repository you don't want included then don't fill in the description. This page will display the repository name, the programming language and the description.

###Create a GitHub Pages repository
Create a new GitHub repository named username.github.io, where username is your username on GitHub.  The first part of the repository must exactly match your username. 

###Create a Readme.md file
It's good practice to have a Readme file for every project. The Readme file  is a good place for a brief summary of your project. This tutorial will work without a readme file but the readme is your way of telling other GitHub users what your project is about.

###Working from the GitHub site
You can complete this tutorial without additional software if you wish to work directly in the browser. Browse to Prose.io and allow Prose to access your GitHub repositories. Then select the GitHub Pages repository you created above. Prose is an IDE that allows you to edit code in a GitHub repository through the browser. You can also edit right through GitHub if you like.

###Working from your computer
If you wish to complete this tutorial from your computer then you need to clone your repository. Open a terminal in your home or workspace folder. 

```
git clone https://username@github.com/username/repositoryname.git
git config set-url origin https://dave45678@github.com/dave45678/DaveWolf
```

**List of other git commands you need**
```
git add --all
git commit -m "commit message"
git pull
git push -u origin master
git status
```

###Create an index page 
Create a web page called index.html.  A minimal HTML5 page is below. You can create this page locally or work directly from GitHub. 
```html
 <!DOCTYPE html>
<html>
<head>
<title>Your Page Title</title>
</head>
<script>
</script>
<body>

</body>
</html> 
```

###Add a link to include jQuery
jQuery is a JavaScript library which simplifies HTML scripting. It's free and open-source. We'll use jQuery to retrieve your repository list. The script below will parse through the JSON-formatted list. It will generate an HTML list of your GitHub projects with descriptions.

Add the script tags for the JQuery library just below the closing head tag. Do not put these in between the other script tags. You'll use them in the next step. 
```html
<!-- jQuery library -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.0/jquery.min.js"></script>
```

Add the following code between script tags or as an external script. I saved mine in a file called listrepos.js which is in a directory I created called scripts.


```javascript
jQuery.githubUser = function(username, callback) {
   jQuery.getJSON('https://api.github.com/users/'+username+'/repos?callback=?',callback)
}
 
//this method
jQuery.fn.loadRepositories = function(username) {
    this.html("<span>Querying GitHub for " + username +"'s repositories...</span>");
     
    var target = this;
    $.githubUser(username, function(data) {
        var repos = data.data; // JSON Parsing
        sortByName(repos);    
     
        var list = $('<dl/>');
        target.empty().append(list);
        $(repos).each(function() {
        	if ((this.description).length>0){
	            if (this.name != (username.toLowerCase()+'.github.com')) {
	                list.append('<dt><a href="'+ (this.homepage?this.homepage:this.html_url) +'">' + this.name + '</a> <em>'+(this.language?('('+this.language+')'):'')+'</em></dt>');
	                list.append('<dd>' + this.description +'</dd>');
	            }
        	}
        });      
      });
      
    function sortByName(repos) {
        repos.sort(function(a,b) {
        return a.name - b.name;
       });
    }
};

```
Next, add the code below to the index.html page. It should appear just before the closing tag for ```</head>```.


```html
<script src="scripts/listrepos.js"></script>
<script>
    $(function() {
        $("#my-github-projects").loadRepositories("dave45678");
    });
</script>
```
Your index page should now show your repositories. You can browse to it by browsing to ```http://username.github.io```.

###Add a Gravatar
Gravatar is a service for providing globally unique avatars. Gravatars can be used on any site. Gravatar works by returning an image for the email address of hte user. The email address must be converted to an MD5 hash.

All URLs on Gravatar are based on the use of the hashed value of an email address. Images and profiles are accessed via the hash of an email. It is considered the primary way of identifying an identity within the system. Follow the following steps to ensure a consistent and accurate hash.
1. Trim leading and trailing whitespace from an email address
2. Force all characters to lower-case
3. Generate an MD5 hash the final string

We're going to use their service to add an avatar to this page. If you don't have an account at Gravatar then visit their site (http://www.gravatar.com) and create one now. 

To test your gravatar simply calculate the MD5 of your email address and browse to https://www.gravatar.com/HASH where HASH is your MD5 hash. 

Once you calculate your hash add ```<img id="gravatar" src="http://gravatar.com/avatar/HASH?s=200" alt="YOUR_NAME"/> ``` to display your Gravatar image to the index.html page. Replace HASH and YOUR_NAME with the MD5 hash and your name.


###Add Bootstrap
Bootstrap is a free and open-source front-end library for creating websites and web applications. It contains HTML- and CSS-based design templates for typography, forms, buttons, navigation and other interface components, as well as optional JavaScript extensions. It aims to ease the development of dynamic websites and web applications.

Bootstrap is a front end web framework, that is, an interface for the user, unlike the server-side code which resides on the "back end" or server.

Since version 2.0 it also supports responsive web design. This means the layout of web pages adjusts dynamically, taking into account the characteristics of the device used (desktop, tablet, mobile phone).

To make your page work with Bootstrap add the following code to your index.html page in the ```<head></head>``` section. Bootstrap requires jQuery but since we added that earlier then it isn't necessary to add it again.

```html
<!-- Bootstrap -->
<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
<!-- Latest compiled JavaScript -->
<script src="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
```

###Add a link to LinkedIn (or your blog, etc)
Professionals all over the world use LinkedIn to manage their professional identity and develop their professional network. You should do the same. If you don't have a linkedIn account then you should create one now. If you'd rather link to a blog or other website that's fine, too.

Creating a link that opens in a new window is as simple as adding an anchor tag to your web page. I added mine just below the image. I used paragraph tags to ensure my link is on its own line.

```html
    <p/>
    <a href="https://www.linkedin.com/in/djwolf" target="_blank">Resume (New Window)</a>
    <p/>
```

###Check out your page
Congratulations on making it this far. Take a moment to browse to your page and check it out. You can then send the link to your friends and let them know how smart you are. The url for your site is ```http://YOUR_NAME.github.io```.

###Setting up a custom domain
You can set up a custom url in for your GitHub Pages site. First you need to register the custom domain with a DNS provider. And then you need to configure your domain with your DNS provider.

###Register a domain
A domain name allows you to create a customized url for your GitHub resume. Getting a domain name involves registering the name with an organization called ICANN. You register through a domain name registrar. I recommend **hover.com**. Once you choose a name you need to go to the hover.com and pay a registration fee that runs from $10 to $35 for most domain names.

###Configure your DNS
You can set up a URL redirect to forward your domain to any destination page of your choice. When you set up a URL redirect for a domain, it may be 30 minutes before the redirect works .

###Add a CNAME file
Create a text file called ```CNAME``` in your GitHub pages site. It should contain one line.. your domain. 

