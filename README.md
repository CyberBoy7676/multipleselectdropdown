# multipleselectdropdown
```python 
#view.py 
from .forms import CityForm
from .models import City 
from . import forms
def citydropdown(request):
    instance = City.objects.all()  #or any queryset from db
    initialValues = [o.id for o in instance]
    form = forms.CityForm(initial={'cityname' == initialValues})
    return render(request,'core/home.html',{"form":form})




#forms.py
from django import forms
from .models import City 
from . import models
class CityForm(forms.ModelForm): 
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

#### this is Error


```python 
Internal Server Error: /
Traceback (most recent call last):
  File "C:\Users\adilr\AppData\Local\Programs\Python\Python39\lib\site-packages\django\core\handlers\exception.py", line 47, in inner
    response = get_response(request)
  File "C:\Users\adilr\AppData\Local\Programs\Python\Python39\lib\site-packages\django\core\handlers\base.py", line 181, in _get_response
    response = wrapped_callback(request, *callback_args, **callback_kwargs)
  File "C:\Users\adilr\django\practice\multipleselect\core\views.py", line 12, in citydropdown
    form = forms.CityForm(initial={'cityname' == initialValues})
  File "C:\Users\adilr\django\practice\multipleselect\core\forms.py", line 11, in __init__
    super(CityForm, self).__init__(*args, **kwargs)
  File "C:\Users\adilr\AppData\Local\Programs\Python\Python39\lib\site-packages\django\forms\models.py", line 287, in __init__
    raise ValueError('ModelForm has no model class specified.')
ValueError: ModelForm has no model class specified.
```

