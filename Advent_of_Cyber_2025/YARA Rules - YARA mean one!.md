# Yara Overview
YARA is a tool built to identify and classify malware by searching for unique patterns, the digital fingerprints left behind by attackers

# Why YARA Matters and When to Use It
YARA gives defenders the power to detect malware by its behavior and patterns, not just by name. YARA allows you to define your own rules, providing your own view of what constitutes "malicious" behavior.

## Situation to use this tool

- Post-incident analysis: when the security team needs to verify whether traces of malware found on one compromised host still exist elsewhere in the environment.
- Threat Hunting: searching through systems and endpoints for signs of known or related malware families.
- Intelligence-based scans: applying shared YARA rules from other defenders or kingdoms to detect new indicators of compromise.
- Memory analysis: examining active processes in a memory dump for malicious code fragments.

# Yara Values
- Speed: quickly scans large sets of files or systems to identify suspicious ones.
- Flexibility: detects everything from text strings to binary patterns and complex logic.
- Control: lets analysts define exactly what they consider malicious.
- Shareability: rules can be reused and improved by other defenders across kingdoms.
- Visibility: helps connect scattered clues into a clear picture of the attack.

# Yara Rules
### Elements 

- Metadata: information about the rule itself: who created it, when, and for what purpose.
- Strings: the clues YARA searches for: text, byte sequences, or regular expressions that mark suspicious content.
- Conditions: the logic that decides when the rule triggers, combining multiple strings or parameters into a single decision.

```
rule TBFC_KingMalhare_Trace
{
    meta:
        author = "Defender of SOC-mas"
        description = "Detects traces of King Malhare’s malware"
        date = "2025-10-10"
    strings:
        $s1 = "rundll32.exe" fullword ascii
        $s2 = "msvcrt.dll" fullword wide
        $url1 = /http:\/\/.*malhare.*/ nocase
    condition:
        any of them
}
```



### Case-insensitive strings - nocase
```
strings:
    $xmas = "Christmas" nocase
```
-> ignore letter casing

### Wide-character strings - wide, ascii
```
strings:
    $xmas = "Christmas" wide ascii
```
Many Windows executables use two-byte Unicode characters. Adding wide tells YARA to also look for this format, while ascii enforces a single-byte search. You can use both together

### XOR strings - xor
Malhare's agents often XOR-encode text to hide it from scanners. Using the xor modifier, YARA automatically checks all possible single-byte XOR variations of a string - revealing what attackers tried to conceal.

```
strings:
    $hidden = "Malhare" xor
```

### Base64 strings - base64, base64wide
Some malware encodes payloads or commands in Base64. With these modifiers, YARA decodes the content and searches for the original pattern, even when it’s hidden in encoded form.
```
strings:
    $b64 = "SOC-mas" base64
```



```

rule TBFC_Simple_MZ_Detect
{
    meta:
        author = "TBFC SOC L2"
        description = "IcedID Rule"
        date = "2025-10-10"
        confidence = "low"

    strings:
        $tbfc_msg = /TBFC:[A-Za-z0-9]+/ ascii

    condition:
        $tbfc_msg
}

```
```
yara -rs icedid_starter.yar  /home/ubuntu/Downloads/easter
TBFC_Simple_MZ_Detect /home/ubuntu/Downloads/easter/easter46.jpg
0x2f78a:$tbfc_msg: TBFC:HopSec
TBFC_Simple_MZ_Detect /home/ubuntu/Downloads/easter/easter16.jpg
0x3bb7f7:$tbfc_msg: TBFC:me
TBFC_Simple_MZ_Detect /home/ubuntu/Downloads/easter/easter52.jpg
0x2a2ad2:$tbfc_msg: TBFC:Island
TBFC_Simple_MZ_Detect /home/ubuntu/Downloads/easter/easter10.jpg
0x137da8:$tbfc_msg: TBFC:Find
TBFC_Simple_MZ_Detect /home/ubuntu/Downloads/easter/easter25.jpg
0x42c778:$tbfc_msg: TBFC:in

```
