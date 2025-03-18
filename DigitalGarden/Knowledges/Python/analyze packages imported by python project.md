
```python
import os
import re

def analyze_imports(project_path):
    imports = set()

    for root, dirs, files in os.walk(project_path):
        for file in files:
            if file.endswith(".py"):
                file_path = os.path.join(root, file)
                with open(file_path, "r") as f:
                    lines = f.readlines()
                    for line in lines:
                        if line.startswith("import"):
                            match = re.findall(r"[\w.]+", line)
                            if match:
                                import_name = match[1].split('.')[0]  # 只保留包名部分
                                imports.add(import_name)
                        elif line.startswith("from"):
                            match = re.findall(r"[\w.]+", line)
                            if match:
                                import_name = match[0].split('.')[0]  # 只保留包名部分
                                imports.add(import_name)

    return imports

project_path = "/path/to/your/project"
result = analyze_imports(project_path)

for import_name in result:
    print(import_name)
```