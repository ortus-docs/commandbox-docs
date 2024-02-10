# Legacy SSL Client Cert syntax

{% hint style="danger" %}
The syntax on this page is deprecated.  It will still work for the foreseeable future, but we recommend using the [new `bindings` object](./) instead which is much more powerful and allows multiple bindings.
{% endhint %}

{% code title="server.json" %}
```javascript
{
  "web" : {
    "ssl" : {
      "enable" : true,
      "clientCert" : {
	"mode" : "Requested",
	"CACertFiles" : "rootCA.cer,anotherRootCA.cer",
	// OR...
	"CACertFiles" : [
    	  "rootCA.cer",
          "anotherRootCA.cer"
	],
	"CATrustStoreFile' : "cacerts",
	"CATrustStorePass' : "changeit"
      }
    }
  }
}
```
{% endcode %}

