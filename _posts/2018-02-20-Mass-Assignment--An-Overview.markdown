
`Mass Assignment:` An attack used on websites that involve some sort of model-binding to a request. The framework binder used for binding the HTTP request parameters to the model class has not been explicitly configured to allow, or disallow certain attributes.

In this article, we'll review what mass assignment is, how it can be a problem, and what can be done to fix it in applications coded in various languages.

![assignment](images/assignment.jpg)

In order to ease development, most modern frameworks allow an object to be automatically instantiated and the HTTP request parameters whose names match an attribute of the class to be bound get automatically populated. This can lead to serious problems if implemented without caution. Any attribute in the bound or nested classes, will be automatically bound to the HTTP request parameters. Therefore, malicious users will be able to assign a value to any attribute in bound or nested classes, even if they are not exposed to the client through web forms or APIs.
The Mass assignment vulnerability shows up every now and then in several popular applications. For example, in 2012, a developer, named Egor Homakov, [took advantage of a security hole at Github](https://medium.com/@homakov/how-i-started-in-web-security-400b80824e86) to gain commit access to the Rails project. This vulnerability can be reproduced in several other websites and languages, thus the need to explicitly map solutions for the same.

`Example:`

Suppose there is a form for editing a user's account information:

{% highlight html %}
<form>
    <input name=username type=text>
    <input name=password type=text>
    <input name=email text=text>
    <input type=submit>
</form>
{% endhighlight %}

The object that the form is binding to is as follows:

{% highlight java %}
  public class User {
     private String username;
     private String password;
     private String email;
     private boolean isAdmin;
     //Getters & Setters
   }
{% endhighlight %}

Here is the controller handling the request:

{% highlight java %}
@RequestMapping(value = "/editUser", method = RequestMethod.POST)
  public String submit(User user) {
     userService.edit(user);
     return "successPage";
  }
{% endhighlight %}

The exploit is:

{% highlight code %}
POST /editUser
..
..
username=malicious&password=somepass&email=malicious@mal.com&isAdmin=true
{% endhighlight %}


## General Solution

#### Create Data Transfer Objects (DTOs):

Create form or DTO objects that have fields for only what is in the form and what should be editable by the users. 

`Example:`
{% highlight java %}
public class UserRegistrationFormDTO {
     private String userid;
     private String password;
     private String email; 
     //NOTE: Sensitive privilege related fields are not present
     //Getters & Setters
   }
{% endhighlight %}

## Framework Specific Solutions

#### ASP .NET MVC:
* `[Bind] Attribute:`
The Bind Attribute lets you whitelist only those properties which should be bound from the incoming request.
{% highlight java %}
[HttpPost]
public ViewResult Edit([Bind(Include = "FirstName")] User user)
{
    // ...
}
{% endhighlight %}
The Bind Attribute can also be used with a blacklist approach.
{% highlight java %}
[HttpPost]
public ViewResult Edit([Bind(Exclude = "IsAdmin")] User user)
{
    // ...
}
{% endhighlight %}

* `UpdateModel and TryUpdateModel API:`
Explicit binding with the UpdateModel and TryUpdateModel API can be performed. These methods also support whitelist and blacklist parameters.
{% highlight java %}
[HttpPost]
public ViewResult Edit()
{
    var user = new User();
    TryUpdateModel(user, includeProperties: new[] { "FirstName" });
    // ...
}
{% endhighlight %}
* `Interface Definitions:`
The controller can be coded as shown below.
{% highlight java %}
[HttpPost]
public ViewResult Edit()
{
    var user = new User();
    TryUpdateModel<IUserInputModel>(user);
    return View("detail", user);
}
{% endhighlight %}
Assuming your interface looks like:
{% highlight java %}
public interface IUserInputModel
{
    string FirstName { get; set; }
}
{% endhighlight %}
The model will have to implement this interface:
{% highlight java %}
public class User : IUserInputModel
{
    public string FirstName { get; set; }
    public bool IsAdmin { get; set; }
}
{% endhighlight %}
* `[ReadOnly] attribute:`
This attribute can be used if binding of a certain property is not required at all.
{% highlight java %}
public class User 
{
    public string FirstName { get; set; }
    [ReadOnly(true)]
    public bool IsAdmin { get; set; }
}
{% endhighlight %}
* `Use two different models:`
Put user input into a model designed for user input only.
{% highlight java %}
public class UserInputViewModel
{
    public string FirstName { get; set; }
}
{% endhighlight %}
#### Spring MVC:
* `Whitelisting:`
Register fields that should be allowed for binding.
{% highlight java %}
@Controller
  public class UserController
  {
     @InitBinder
     public void initBinder(WebDataBinder binder, WebRequest request)
     {
        binder.setAllowedFields(["userid","password","email"]);
     }
     ...
  }
{% endhighlight %}
* `Blacklisting:`
Register fields that should not be allowed for binding.
{% highlight java %}
@Controller
  public class UserController
  {
     @InitBinder
     public void initBinder(WebDataBinder binder, WebRequest request)
     {
        binder.setDisallowedFields(["isAdmin"]);
     }
     ...
  }
{% endhighlight %}
#### Ruby on Rails:
* `attr_protected (Blacklist Approach):`
The attr_protected method takes a list of attributes that will not be accessible for mass-assignment.

**Example:**

{% highlight ruby %}
attr_protected :admin
(or)
attr_protected :last_login, :as => :admin
{% endhighlight %}
Where “admin” is the role option. If no role is defined then attributes will be added to the “:default” role.
* `attr_accessible (WhiteList Approach):`
This accepts a list of attributes that will be accessible for mass-assignment. All other attributes will be protected.

**Example:**

{% highlight ruby %}
attr_accessible :name attr_accessible :name, :is_admin, :as => :admin
{% endhighlight %}
The most secure technique to protect your whole project would be to enforce that all models define their accessible attributes. This can be achieved with an application config option:
{% highlight ruby %}
config.active_record.whitelist_attributes = true
{% endhighlight %}
This will create an empty whitelist of attributes available for mass-assignment for all models in your application. As such, your models will need to explicitly whitelist or blacklist accessible parameters by using an attr_accessible or attr_protected declaration. (Note:  This technique is best applied at the start of a new project.)
#### Django:
Create forms to avoid Mass assignment. All fields that are intended for inclusion in the form should be listed explicitly in the “fields” attribute.
{% highlight django %}
from django import forms
from myapp.models import Whatzit
class WhatzitForm(forms.ModelForm):
    class Meta(object):
        model = Whatzit
        fields = ('foo', 'bar', 'baz')
{% endhighlight %}

### In Conclusion..

Mass assignment can be an incredibly useful feature when writing code. However, the consequences of a Mass Assignment vulnerability can be serious and mass assignment security is really about handling untrusted input. A developer can protect an application by following some basic coding best practices such as:
* Whitelist the bindable, non-sensitive fields
* Blacklist the non-bindable, sensitive fields
* Use Data Transfer Objects (DTOs)

### References

* [Mass Assignment Vulnerability](https://en.wikipedia.org/wiki/Mass_assignment_vulnerability)
* [Mass Assignment Cheat Sheet](https://www.owasp.org/index.php/Mass_Assignment_Cheat_Sheet)
* [Fortify Taxonomy – Insecure Binder Configuration](https://vulncat.hpefod.com/en/detail?id=desc.dataflow.java.mass_assignment_insecure_binder_configuration)
* [http://guides.rubyonrails.org/v3.2.9/security.html#mass-assignment](http://guides.rubyonrails.org/v3.2.9/security.html#mass-assignment)
* [https://docs.djangoproject.com/en/1.11/releases/1.6/](https://docs.djangoproject.com/en/1.11/releases/1.6/)


