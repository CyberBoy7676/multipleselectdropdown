# multipleselectdropdown
```python 
#view.py 
from .forms import CityForm
from .models import City 
from . import forms
def citydropdown(request):
    instance = City.objects.all()  #or any queryset from db
    initialValues = [o.id for o in instance]
    form = forms.CityForm(initial={'cityname' : initialValues})
    return render(request,'core/home.html',{"form":form})




#forms.py
from django import forms
from .models import City 
from . import models
class CityForm(forms.Form): 
    def __init__(self, *args, **kwargs):
        CHOICES = []
        for o in models.City.objects.all():
            CHOICES.append((o.id, o.cityname))  #1st value in tuple: the value sent as form data | 2nd value in tuple: placeholder for data
        super(CityForm, self).__init__(*args, **kwargs)
        self.fields['cityname'] = forms.MultipleChoiceField(choices=CHOICES, widget=forms.SelectMultiple(attrs={'class':'select2'}))



#models.py
from django.db import models

class City(models.Model):
    cityname=models.CharField(max_length=100)


```

