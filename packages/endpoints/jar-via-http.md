# Jar (via HTTP)

If you have external jars that need downloaded into your project, you can use the `jar:` endpoint to download them.  The jar endpoint does not expect the jars to be contained in a zip file or to have a box.json.  As such, there is no real package slug or name, so CommandBox will "guess" the name based on the name of the jar (if a jar name appears in the path or query string).

