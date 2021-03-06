---
layout: loop
title: Authentification checker loop
description:
    - The Auth loop perform authorisation checks against the current user in the front-office or back-office context. This loop returns nothing if the authaurization fails, or the loop contents if it succeds.
    - You may check in the front office if an administrator is logged in, and perform specific functions in your front-office template (such as direct editing, for example).
sidebar: loop
lang: en
subnav: loop_auth
uses_global_argument: false
returns_global_outputs: { countable : false, timestampable : false, versionable : false }
type: auth
arguments :
    - {name: "context", 
       description: "Defines the authorisation check context", mandatory="false", 
       default="front", 
       expected_values: [
          {name: "front", description: "Front office context. Use the customer user to perform the check."},
          {name: "admin", description: "The back-office context. Uses the admin user to perform the check."}
       ]}
    - {name: "role", description: "A comma separated list of user roles", mandatory: "true"}
    - {name: "resource", description: "A comma separated list of resources. If empty or missing, the authorization is checked against the roles only"}
    - {name: "access", description: "A comma separated list of accesss. If empty or missing, the authorization is checked against the roles only",
        expected_values: [
            {name: "view"},
            {name: "update"},
            {name: "create"},
            {name: "delete"}
        ]
    }
---

<div class="description large-12">
    I want to check if current administrator is allowed tu use the back-office search function.
</div>

<div class="code large-12">

{% highlight smarty %}

{loop name="top-bar-search" type="auth" context="admin" roles="ADMIN" permissions="admin.search"}
    <form class="form-search" action="{url path='/admin/search'}">
        ... form content ...
	</form>
{/loop}


{% endhighlight %}

</div>&nbsp;

 <div class="postscriptum large-12">

    The context is "admin", which means that the Administrator user is used to check the authorisation.
    The role is ADMIN, which mean that the current user should have the "ADMIN" role.
    The permission is "admin.search", which is the identifier of the search permission.

</div>

<div class="description large-12">
    I want to check if the customer is logged in, or not.
</div>

<div class="code large-12">

{% highlight smarty %}

{loop type="auth" name="customer_info_block" roles="CUSTOMER" context="front"}
    <p>Your are logged in. <a href="{viewurl view='index' action='logoutCustomer'}">Logout</a></p>
{/loop}

{elseloop rel="customer_info_block"}
    You are not logged in. <a href="{viewurl view='login'}">Login now</a> or <a href="{viewurl view='create_account'}">create your account</a>
{/elseloop}


{% endhighlight %}

</div>&nbsp;