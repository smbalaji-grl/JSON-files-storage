# JSON Configuration Files for C3 Browser Application

## Overview

This repository contains the **production JSON configuration files** for the C3 Browser Application Exerciser Mode. These are the files directly used by the application.

## Files

| File | Description | Used By |
|------|-------------|---------|
| `PRx-PTxPacketFormat.json` | Complete packet definitions (PRx & PTx) | Backend - Packet definitions |
| `ExtendedConfigurationTPR.json` | TPR phase parameters (timing, config) | Backend - TPR mode configuration |
| `ExtendedConfigurationTPT.json` | TPT phase parameters (timing, config) | Backend - TPT mode configuration |
| `ConfigDefaultPktTPR.json` | TPR default packet sequences | Backend - C3BrowserApp_BE/Json_Helper |
| `ConfigDefaultPktTPT.json` | TPT default packet sequences | Backend - C3BrowserApp_BE/Json_Helper |
| `fields-applicable-data.json` | Field applicability matrix | Backend - Field validation |

## Maintenance

### ✅ Direct JSON Editing (Recommended)

For most updates, **edit these JSON files directly**:

```bash
# Edit the file
code ExtendedConfigurationTPR.json

# Check changes
git diff ExtendedConfigurationTPR.json

# Commit
git add ExtendedConfigurationTPR.json
git commit -m "Fix: Updated t_Start default value for BPP_EPP"
git push
```

### When to Edit Each File

**PRx-PTxPacketFormat.json**
- Adding new packet definitions
- Modifying packet fields
- Updating byte configurations
- Changing field properties

**ExtendedConfigurationTPR.json / TPT.json**
- Updating phase parameter defaults
- Adding new phase parameters
- Modifying field validation ranges
- Changing UI display strings

**ConfigDefaultPktTPR.json / TPT.json**
- Updating default packet sequences
- Modifying context constraints (CoilType, FrequencyMode, PowerProfile)
- Changing default field values
- Adding version-specific sequences

## Common Updates

### Update Phase Parameter Default Value

**File:** `ExtendedConfigurationTPR.json` or `ExtendedConfigurationTPT.json`

```json
{
  "FieldName": "t_Start",
  "FieldDisplayString": "t Start",
  "DefaultValue": "15",  // ← Change this
  "FieldType": "textbox",
  "Units": "ms",
  "MinValue": 0,
  "MaxValue": 255,
  // ...
}
```

### Add Context Constraint

**File:** `ConfigDefaultPktTPR.json` or `ConfigDefaultPktTPT.json`

```json
{
  "PRxQIPacketName": "Signal Strength",
  "PTxQIPacketName": "",
  "ContextConstraints": {
    "CoilType": ["TPR_1A", "TPR_1B", "TPR_1C"]  // ← Add more coil types
  },
  // ...
}
```

### Add New Packet

**File:** `PRx-PTxPacketFormat.json`

Find the appropriate version section and add:

```json
{
  "QIPacketName": "Your New Packet",
  "Header": "0x99",
  "Selector": "",
  "ByteCount": 1,
  "Mnemonic": "YNP",
  "PacketPhase": "ID",
  "Fields": [
    {
      "FieldName": "Your_Field",
      "Units": "",
      "NumberStyle": "int",
      // ...
    }
  ],
  "ExtendedConfiguration": []
}
```

## Validation

After editing, validate JSON syntax:

```bash
# Using Python
python -m json.tool ExtendedConfigurationTPR.json > /dev/null

# Using jq (if installed)
jq empty ExtendedConfigurationTPR.json

# Using Node.js
node -e "JSON.parse(require('fs').readFileSync('ExtendedConfigurationTPR.json'))"
```

## Documentation References

- [Field Applicability Matrix](../c3-sw-docs/C3-SW-Docs/Features/Exerciser%20Mode/Rev4/ServerClientTransctionClassModel/ClientFieldUpdateDocumentation.md)
- [Default Packets Mapping](../c3-sw-docs/C3-SW-Docs/Features/Exerciser%20Mode/DefaultPackets_Mapping.md)
- [Exerciser Mode HLD](../c3-sw-docs/C3-SW-Docs/Features/Exerciser%20Mode/ExerciserModeHLD.md)

## Excel/Python Generation (Reference Only)

For **major overhauls** (adding many packets at once, restructuring), you can use the Excel-based generation:

```bash
cd ../Excersiermode_automation
python generate_all_json_from_excel.py
# Copy generated files back to JSON-files-storage/
```

**Note:** This is rarely needed. For normal updates, edit JSON directly.

## Git Workflow

```bash
# Check status
git status

# See what changed
git diff

# Stage changes
git add ExtendedConfigurationTPR.json ExtendedConfigurationTPT.json

# Commit with descriptive message
git commit -m "Update BPP_EPP phase parameters per spec v2.2.1"

# Push to remote
git push
```

## Best Practices

✅ **DO:**
- Edit JSON directly for small changes
- Validate JSON syntax before committing
- Write clear commit messages
- Test changes in development environment first
- Review diffs before pushing

❌ **DON'T:**
- Commit invalid JSON (breaks application)
- Make large changes without backup
- Skip validation step
- Commit without testing

## Support

For questions or issues:
- Check documentation in `c3-sw-docs/`
- Contact backend team
- Slack: #exerciser-mode-config

---

**Last Updated:** 2025-10-30  
**Maintenance Mode:** Direct JSON editing  
**Status:** ✅ Production files

