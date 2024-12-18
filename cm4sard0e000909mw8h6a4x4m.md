---
title: "How to Validate Rectangular Images in Django Using Python"
datePublished: Tue Dec 17 2024 10:04:25 GMT+0000 (Coordinated Universal Time)
cuid: cm4sard0e000909mw8h6a4x4m
slug: how-to-validate-rectangular-images-in-django-using-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734429833724/dfb71fdd-31dc-48e1-8e30-5172678b8951.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1734429854032/16f1c0b7-1705-4d96-999d-50c02670d462.webp
tags: image-processing, python, django

---

When working with image uploads in a Django project, there may be situations where you need to enforce specific dimensions, such as ensuring the uploaded image is **rectangular** (not square). This can be particularly useful for profile headers, banners, or media requiring non-square formats.

In this article, we’ll walk through a simple solution using Django's validation system and the **Pillow** library.

## Prerequisites

Before implementing the solution, ensure you have the following dependencies installed:

1. **Django** (for web framework functionality)
    
2. **Pillow** (for image processing)
    

If you don’t have Pillow installed, you can add it using:

```python
python -m pip install pillow
```

# Writing the Validator

To validate whether an uploaded image is rectangular, we need to check the **width** and **height** of the image. If both dimensions are equal, it means the image is square, and we’ll raise a validation error.

Here’s the code for the custom validator:

```python
from django.core.exceptions import ValidationError
from PIL import Image

def validate_rectangular_image(image):
    """
    Validator to ensure an uploaded image is rectangular and not square.
    """
    image = Image.open(image)  # Open the uploaded image using Pillow
    width, height = image.size  # Extract dimensions
    
    if width == height:  # Check if image is square
        raise ValidationError("Uploaded image must be rectangular (not square).")
    
    return image
```

## Integrating the Validator with a Django Model

To use this validator in your Django application, you can add it to a model field. For instance, let’s assume you have an ImageField in a model for a user profile banner:

```python
from django.db import models
from .validators import validate_rectangular_image  # Import the custom validator

class Profile(models.Model):
    name = models.CharField(max_length=100)
    banner_image = models.ImageField(
        upload_to='banners/', 
        validators=[validate_rectangular_image],
        help_text="Please upload a rectangular image for the banner."
    )
    
    def __str__(self):
        return self.name
```

### How It Works:

* The `validate_rectangular_image` function is called whenever a file is uploaded to the `banner_image` field.
    
* If the image is square, a `ValidationError` is raised, preventing the file from being saved.
    
* Only rectangular images will pass validation and be uploaded successfully.
    

## Handling Validation Errors in Forms

If you’re using Django forms for image uploads, the error will be displayed to users when they submit an invalid image.

For example, a simple form could look like this:

```python
from django import forms
from .models import Profile

class ProfileForm(forms.ModelForm):
    class Meta:
        model = Profile
        fields = ['name', 'banner_image']
```

When a user uploads a square image, they will see the error message:

> **"Uploaded image must be rectangular (not square)."**

## Testing the Validator

You can test the functionality by trying to upload both square and rectangular images.

1. **Square Image (e.g., 300x300):**  
    The validator will reject the file and raise a `ValidationError`.
    
2. **Rectangular Image (e.g., 400x300):**  
    The validator will accept the file, and the image will be uploaded successfully.
    

## Conclusion

By using this approach, you can enforce image dimension requirements seamlessly in your Django applications. The `Pillow` library makes it easy to work with image sizes, and Django's validation system allows you to integrate custom logic without much effort.

### Key Takeaways:

* Use **Pillow** to extract image dimensions.
    
* Raise `ValidationError` when an uploaded image fails your criteria.
    
* Integrate validators into Django models to ensure data integrity.
    

---

By combining Django and Pillow, you can create powerful and flexible image upload rules that enhance the quality of your web applications.

Happy coding! 🚀