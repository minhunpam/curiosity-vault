- an isolated environment on your computer, where you can run and test your Python projects
- Python has the built-in `venv` module for creating virtual environments

- Create a virtual environment named `myFirstProject`:
```bash
python3 -m venv myFirstProject
```

- Activate virtual environment:
```bash
source myFirstProject/bin/activate
```
- After activation, your prompt will change to show that you are now working in the active environment
```bash
(myFirstProject) C:\Users\Your Name>
```

- Deactivate Virtual Environment
```bash
(myfirstproject) ... $ deactivate
```

- Delete Virtual Environment
```bash
rm -rf myFirstProject
```

