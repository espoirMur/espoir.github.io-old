
first of all get the current working directory and save it :

old_path = os.getcwd()

the change the cuurent working directory to the directory you are looking in:

os.chdir("../../directory/")

retrieve the files with :

os.listdir()

restore the working directory to the old one :

os.chdir(old_path)
