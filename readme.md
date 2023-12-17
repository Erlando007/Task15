
1. **Создайте Django-проект:**
   
   В вашем терминале выполните следующие команды:

   ```bash
   django-admin startproject config
   ```

2. **Создайте приложения `ManytoManyApp`:**
   
   ```bash
   python3 manage.py startapp ManytoManyApp
   ```

3. **Определите модель Student:**

   ```python
   class Student(models.Model):
        name = models.CharField(max_length=100)
        age = models.IntegerField()

   ```

4. **Определите модель Teacher:**

   ```python
  class Teacher(models.Model):
        name = models.CharField(max_length=100)
        subject = models.CharField(max_length=40)
        experience = models.IntegerField()
        students = models.ManyToManyField(Student, related_name='teachers')

   ```

5. **Создайте приложения `OnetoOneApp`:**
    ```bash
   python3 manage.py startapp OnetoOneApp
   ```

6. **Определите модель Brain:**

   ```python
   class Brain(models.Model):
        iq = models.IntegerField()
        weight = models.IntegerField()

   ```

7. **Определите модель Human:**

   ```python
  class Human(models.Model):
    SEX = (
        ('male', 'Мужчина'),
        ('female', 'Женщина'),
        ('think', 'Неопределенный'),
        ('flight helicopter', 'Боевой вертолетик')
    )

    name = models.CharField(max_length=50, default='John')
    sex = models.CharField(max_length=20, choices=SEX)
    brain = models.OneToOneField(Brain, on_delete=models.CASCADE,related_name='human')

    ```

8. **Настройте приложения в `settings.py`:**

   В файле `config/settings.py` добавьте в раздел `INSTALLED_APPS` ваши приложения:

   ```python
   INSTALLED_APPS = [
       # ...
        'rest_framework',
        'ManytoManyApp',
        'OnetoOneApp'
   ]
   ```

9. **Выполните миграции:**

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

10. **Работа с Shell:**
################ Создание и работа с обьектом (ManytoMany)########################

>>>  student1 = Student.objects.create(name='student1', age=15)
>>>  student2 = Student.objects.create(name='student2', age=16)
>>>  student3 = Student.objects.create(name='student3', age=17)

>>>  teacher1 = Teacher.objects.create(name='teacher1', subject='math', experience=2)

>>>  teacher1.students.set([student1,student2,student3]) # обращаемся к связи и добавляем новых студентов
>>>  student1.teachers.all() # получение списка учетелей у одного студента
# <QuerySet [<Teacher: Teacher object (1)>]>

>>>  teacher2 = Teacher.objects.create(name='teacher2', subject='bio', experience=1)
>>>  teacher2.students.set([student1,student3]) # обращаемся к связи и добавляем новых студентов
>>>  teacher2.students.all() # получение списка студентов у одного учителя
# <QuerySet [<Student: Student object (1)>, <Student: Student object (3)>]>

>>>  student1.teachers.all()
# <QuerySet [<Teacher: Teacher object (1)>, <Teacher: Teacher object (2)>]>

>>>  student2.teachers.all()
# <QuerySet [<Teacher: Teacher object (1)>]>


################ Создание и работа с обьектом (OnetoOne)########################

>>>  brain1 = Brain.objects.create(iq='100', weight='5')
>>>  brain2 = Brain.objects.create(iq='150', weight='8')


>>>  human1 = Human.objects.create(sex='Мужчина', brain1=brain) # создаем человека и создаем связь
>>>  human2 = Human.objects.create(sex='Женщина', brain2=brain) # создаем человека и создаем связь

############### Поиск, Изменение , Сохранение а также Удаление обьектов ################

>>>  brain1 = Brain.objects.get(iq='100') # Поиск обьекта по определенным критерями
>>>  brain1.weight='6' # Изменение определенного критерия 
>>>  brain1.save() # Сохранение изменений
>>>  brain1.delete() # Удаление обьекта

############### Использование агрегирующих функций и операторов #########################

>>> totalcount = Brain.objects.count()
>>> totalcount
0 # результат функции

>>> objects_gt = Brain.objects.filter(iq__gt=5) # фильтрация с оператором больше чем
>>> objects_gt = Brain.objects.filter(iq__lt=20) # фильтрация с оператором меньше чем

