# Pretty Diagrams

This is what a typical ColdFusion deployment looks like.  A separate web server sites in front, bound to ports 80 and 443 and proxies back to a Tomcat-based servlet:

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Typical Servlet Deployment</p></figcaption></figure>

## CommandBox's Servlet configuration

For a CommandBox server, we replace the green box above with JBoss Undertow instead of Tomcat and we configure it on the fly with CommandBox.  Here's a look under the covers at what the internal pieces of CommandBox look like for a single site server.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Inside an Undertow-powered CommandBox single-site server</p></figcaption></figure>

We have a set of HTTP/SSL/AJP listeners that feed through a single handler chain of configuration that funnel requests to the servlet deployment where the CF engine's libs are loaded.  The default servlet serves static files via the singular resource manager configured to look in the web root.

## CommandBox Multi-Site with Lucee

Here is what a multi-site server looks like under the covers for Lucee Server (which works differently than Adobe).  This is mostly the same regardless of whether you're using ModCFML or the new Multi-site features.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Inside an Undertow-powered CommandBox multi-site Lucee server</p></figcaption></figure>

Here the listeners funnel to a handful of different initial handler chains, which represent the different configuration between sites.  The Lucee jars are loaded once and the Lucee Servlet Context exists once.  Each site has a separate servlet deployment where a separate Lucee Web Context lives, along with their own CFML Servlet, default servlet, and resource manager configured for their separate web root.

In Multi-Site mode, static files are NOT served by the default servlet, but  instead by an Undertow resource handler in the initial handler chain.  The `web.servletPassPredicate` controls what requests even reach the servlet.  Static files are served without ever touching the servlet.

## CommandBox Multi-Site with Adobe CF

Here is what a multi-site server looks like under the covers for Adobe ColdFusion (which works differently than Lucee).  This is the mostly the same regardless of whether you're using ModCFML or the new Multi-site features.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Inside an Undertow-powered CommandBox multi-site ACF server</p></figcaption></figure>

Like the Lucee example above, the listeners pass requests off to one of several initial handler chains, which represent different per-site configurations.  However, we only have a single servlet deployment in this case and a single default servlet. The resource manager delegates out to sub-resource managers based on the site being accessed to switch out the web root as necessary.

In Multi-Site mode, static files are NOT served by the default servlet, but  instead by an Undertow resource handler in the initial handler chain.  The `web.servletPassPredicate` controls what requests even reach the servlet.  Static files are served without ever touching the servlet.
