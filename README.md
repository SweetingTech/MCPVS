# MCPVS - Model Context Protocol Vulnerability Scanner

A lightweight, extensible command-line tool for scanning code repositories and identifying potential security vulnerabilities using pattern matching.

## Overview

MCPVS scans source code directories, matching file contents against a database of security rules and patterns. It uses a local SQLite database to store rules and provides a mechanism to keep these rules updated from a remote source.

> 🔐 Originally conceived for detecting vulnerabilities in model context protocols, MCPVS has evolved into a general-purpose code scanning tool.

## Features

- **Local Scanning**: Scan downloaded code repositories directly on your machine
- **Rule-Based Detection**: Uses regex and keyword matching to identify potential issues
- **Multi-Language Support**: Works with Python, JavaScript, Java, Shell scripts, and more
- **Lightweight SQLite Backend**: No need for complex database setup
- **Automatic Updates**: Keep your vulnerability rules current with a simple update command
- **Extensible Architecture**: Add new rule types and languages as needed

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/mcpvs.git
cd mcpvs

# Install dependencies
pip install -r requirements.txt

# Install MCPVS (development mode)
pip install -e .
```

## Usage

```bash
# Initialize the database (first run only)
mcpvs init

# Scan a code repository
mcpvs scan /path/to/downloaded/repo

# Update the rules database
mcpvs update-rules

# Show program help
mcpvs --help
```

## Project Structure

```
mcpvs/
├── mcpvs_main.py       # Main script, CLI handler
├── scanner.py          # Core scanning logic
├── database.py         # SQLite interactions (setup, querying)
├── updater.py          # Rule update logic
├── rules/              # Folder to store initial rules definition
│   └── initial_rules.json
└── data/               # Where the SQLite DB will live (or ~/.config/mcpvs)
    └── rules.db
```

## Rule Format

Rules are stored in a SQLite database with the following schema:

```sql
CREATE TABLE scan_rules (
    rule_id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL UNIQUE,
    description TEXT,
    pattern_type TEXT NOT NULL,
    pattern TEXT NOT NULL,
    language TEXT DEFAULT 'any',
    severity TEXT DEFAULT 'info',
    cwe INTEGER,
    reference_url TEXT,
    enabled INTEGER DEFAULT 1
)
```

### Sample Rule

```json
{
  "name": "Python_Eval_Usage",
  "description": "Use of eval() can execute arbitrary code.",
  "pattern_type": "keyword",
  "pattern": "eval(",
  "language": "python",
  "severity": "high"
}
```

## Future Enhancements

- AST-based scanning for more accurate results
- Web dashboard for visualization
- Support for dependency scanning (requirements.txt, package.json)
- Code context analysis for reduced false positives
- Integration with CI/CD pipelines

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

Made with ❤️ by SweetingTech
