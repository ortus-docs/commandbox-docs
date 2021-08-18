# Usage

Once you have registered your endpoint and you are authenticated, you are able to install packages from your own endpoint like this:  
`install stg-forgebox:coldbox`

![](../.gitbook/assets/forgebox-endpoint-default-list%20%281%29.gif)

In the previous example you can see we are installing a `coldbox` package from `stg-forgebox`  which is our own endpoint instead of the default one.

{% hint style="info" %}
You can set your own endpoint as default and then you will not need to use the namespace to refer to your endpoint. In the following example, we will ask CommandBox to retrieve a `coldbox` package from our own endpoint without using a namespace

example: `install coldbox`
{% endhint %}

