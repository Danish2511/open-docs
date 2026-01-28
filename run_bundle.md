```python
import re

def extract_keys_from_properties_block(properties_str):
    keys = set()
    for line in properties_str.strip().splitlines():
        line = line.strip()
        if not line or line.startswith("#") or "=" not in line:
            continue
        key = line.split("=", 1)[0].strip()
        keys.add(key)
    return keys

def extract_template_keys_from_yaml():
    pattern = re.compile(r"\{\{\s*([\w\.-]+)\s*}}")
    template_keys = set()
    source_dir = "sources/yaml"
    for root, _, files in os.walk(source_dir):
        for filename in files:
            filepath = os.path.join(root, filename)
            with open(filepath, 'r', encoding='utf-8') as f:
                content = f.read()
                matches = pattern.findall(content)
                template_keys.update(matches)
    return template_keys

def extract_env_keys(yaml_file):
    keys = set()
    if not os.path.exists(yaml_file):
        return keys
    with open(yaml_file, 'r', encoding='utf-8') as f:
        lines = f.readlines()
        for line in lines:
            match = re.match(r'^\s*([\w\.-]+)\s*:', line)
            if match:
                keys.add(match.group(1))
    return keys

def generate_comparison_report():
    common_keys = extract_keys_from_properties_block(common_properties)
    template_keys = extract_template_keys_from_yaml()
    template_keys = template_keys - common_keys

    env_paths = {
        "dv": f"variables/dv/properties/{bundle_info['name']}-prop.yaml",
        "ts1": f"variables/ts1/properties/{bundle_info['name']}-prop.yaml",
        "qa": f"variables/qa/properties/{bundle_info['name']}-prop.yaml",
        "pd": f"variables/pd/properties/{bundle_info['name']}-prop.yaml"
    }

    report_lines = []
    report_lines.append("Key Validation Report")
    report_lines.append("============================================")
    report_lines.append(f"Total template keys (excluding common properties): {len(template_keys)}")

    for env, path in env_paths.items():
        env_keys = extract_env_keys(path)
        missing = sorted(template_keys - env_keys)
        report_lines.append(f"\nMissing keys in [{env}] ({len(missing)}):")
        report_lines.extend([f"  - {key}" for key in missing])

    report = "\n".join(report_lines)

    report_file = ".support/key-comparison-report.txt"
    writeToFile(report, report_file)
    print(f"\n✅ Key comparison report written to {report_file}")

generate_comparison_report()
print (f'')
print (f'----------------------------------------------------------------------------------')

def extract_mounts_from_bundle(bundle_yaml_path="bundle.yaml"):
    """Extract the list of mounts from bundle.yaml"""
    if not os.path.exists(bundle_yaml_path):
        return []
    with open(bundle_yaml_path, "r", encoding="utf-8") as f:
        data = yaml.safe_load(f) or {}
        return data.get("mounts", [])

def list_files_in_mounts(mount_dir="mounts"):
    """List all files inside the mounts directory (non-recursive)"""
    if not os.path.exists(mount_dir):
        return []
    return [f for f in os.listdir(mount_dir) if os.path.isfile(os.path.join(mount_dir, f))]

def validate_mounts(mount_dir="mounts", bundle_yaml_path="bundle.yaml"):
    mounts_in_bundle = set(extract_mounts_from_bundle(bundle_yaml_path))
    files_in_mount_dir = set(list_files_in_mounts(mount_dir))

    # Files in mounts dir but missing in bundle.yaml
    missing_in_bundle = sorted(files_in_mount_dir - mounts_in_bundle)

    # Entries in bundle.yaml but file not present in mounts dir
    extra_in_bundle = sorted(mounts_in_bundle - files_in_mount_dir)

    report_lines = []
    report_lines.append("Mounts Validation Report")
    report_lines.append("============================================")
    report_lines.append(f"Total files in mounts/: {len(files_in_mount_dir)}")
    report_lines.append(f"Total mounts in bundle.yaml: {len(mounts_in_bundle)}")

    report_lines.append(f"\n❌ Missing in bundle.yaml ({len(missing_in_bundle)}):")
    report_lines.extend([f"  - {f}" for f in missing_in_bundle])

    report_lines.append(f"\n⚠️ Extra in bundle.yaml but not in mounts/ ({len(extra_in_bundle)}):")
    report_lines.extend([f"  - {f}" for f in extra_in_bundle])

    report = "\n".join(report_lines)

    report_file = ".support/mounts-validation-report.txt"
    os.makedirs(os.path.dirname(report_file), exist_ok=True)
    with open(report_file, "w", encoding="utf-8") as f:
        f.write(report)

    # print(report)
    print(f"\n✅ Mounts validation report written to {report_file}")

# Run
validate_mounts()

import pyperclip  # install with: pip install pyperclip

# Copy to clipboard
pyperclip.copy(camel_command)
print("\n✅ Command copied to clipboard!")
```
