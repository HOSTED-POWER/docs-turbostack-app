---
hidden: true
---
Install dependencies required by Odoo and your custom modules using `pip`.

## Odoo Core Requirements

```
cd ~/application
pip install -r odoo/requirements.txt
```

## Custom Addons
If your custom modules include their own `requirements.txt`:

```
pip install -r custom-addons/*/requirements.txt
```

## Best Practices
- Use the correct `pip` from your virtual environment.
- Avoid installing system-wide.