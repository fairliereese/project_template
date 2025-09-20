Progress and testing for cookiecutter-ifying the template


* Goal : global imports which enable whatever directory structure you want

 this works!!
```bash
cookiecutter project_template --no-input
```

does not work
<!-- Let's try now to recursively run this to create new users
```bash
cookiecutter project_template \
  --no-input \
  project_name='test_cookiecutter' # hopefully this comb. of args will only fill in project name for now
``` -->

I will now try with user: \{\{cookiecutter.user\}\} in the replacement LOL
```bash
cookiecutter project_template --no-input
```

I am now going to try doing this iteratively. I want to run one cookie cutter just with the project_template module, then one with differnt values for each user

```bash
cd /Users/fairliereese/Documents/programming/mele_lab/dev/
git clone git@github.com:fairliereese/project_template.git
cookiecutter project_template --no-input project_name=test_cookiecutter
# cookiecutter gh:fairliereese/project_template --no_input project_name=test_cookiecutter # should be equivalent but we'll use the other strat for now bc of 1. branches and 2. eventual better control over submodule treadment
```

Now, from the template user "submoduel" (not one yet oficialy)
```bash
project_name=test_cookiecutter
user=test_user

cd /Users/fairliereese/Documents/programming/mele_lab/dev/
git clone git@github.com:fairliereese/project_template.git
cookiecutter project_template --no-input project_name=$project_name

cd /Users/fairliereese/Documents/programming/mele_lab/dev/
git clone git@github.com:fairliereese/template_user.git
cookiecutter /Users/fairliereese/Documents/programming/mele_lab/dev/template_user \
  --no-input \
  project_name=$user \
  project_name_parent=$project_name \
  user=$user

```


Ok now maybe we should try all together, but with no submodules still
```bash
set -e

project_name=test_cookiecutter
user=test_user

cd /Users/fairliereese/Documents/programming/mele_lab/dev/test/

git clone git@github.com:fairliereese/project_template.git

cookiecutter project_template \
  --no-input \
  project_name=$project_name

cd $project_name/$project_name  

git clone git@github.com:fairliereese/template_user.git
cookiecutter template_user \
  --no-input \
  project_name=$user \
  project_name_parent=$project_name \
  user=$user

cd ../
pip install -e .
```

Okay, now I'm gonna try to get the import to work but w/ R
I found about this library in R called here() which finds the project root by looking
for .git / DESCRIPTION / .Rproj files in the project root, so this *should* do the trick

```R
install.packages('here')
library(here)
```
