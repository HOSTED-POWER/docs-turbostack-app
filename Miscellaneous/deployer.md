---
hidden: true
---

# Prepare your home directory for versioned releases

In this article, we'll guide you through replacing the default public_html directory with a 'releases'-based setup, perfect for your CI/CD setup.

## Step-by-step guide

1. Remove the existing public_html directory (make sure to check that it's empty, this command will fail if not)

```bash
rmdir ~/public_html
```

2. Either create the desired directories, or push a deploy of your code if it's already contained in the right directory setup. We'll assume your code already is set-up in its directories, that look something like this:

```bash
current -> releases/<current-release>
releases/
shared/
```

As you can see, we're assuming the 'current' directory is the symlink pointing to the most current release and this gets updated by your CI/CD deploy.

3. We're now gonna create a symlink to connect 'public_html' to the actual root directory, through the 'current' symlink:

```bash
ln -s current public_html
```

4. At this point, you should be done, but verify that all symlinks are looking fine:

```bash
ls -l
<...>
current -> releases/<current-release>
public_html -> current
releases/
shared/
<...>
```

## Common issues
Of course, there's plenty similar but different frameworks out there that might require a different set-up. For example, it's possible that the current & releases directory are themselves inside a different directory, like 'application'. In such scenario, you simply create the final symlink in step 3, replacing 'current' with the path to the current directory. In this exampe that would become:

```bash
ln -s application/current public_html
```