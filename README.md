# BackupFinder

Good Day!

I truly hope everything is awesome on your side of the screen! 😊

**BackupFinder** discovers backup files on web servers by generating intelligent patterns.  
It creates thousands of potential backup file names based on your target domain.  
Perfect for penetration testing, bug bounty hunting, and security audits.

---

## Requirements

- Go 1.21 or higher
- Internet connection for installation

---


## 🚀 Latest Install Command (Recommended)

```bash
go install github.com/MuhammadWaseem29/BackupFinder/cmd/backupfinder@v1.0.1
```

## Installation

### Install via Go (Recommended)

```bash
# One-command install (always works)
go install github.com/MuhammadWaseem29/BackupFinder/cmd/backupfinder@v1.0.1
```

### If Installation Fails

```bash
# Clear cache and retry
go clean -modcache
go install github.com/MuhammadWaseem29/BackupFinder/cmd/backupfinder@v1.0.1
```

### Alternative Installation Methods

```bash
# Use specific version
go install github.com/MuhammadWaseem29/BackupFinder/cmd/backupfinder@v1.0.0

# Use main branch  
go install github.com/MuhammadWaseem29/BackupFinder/cmd/backupfinder@main
```

### Manual Installation

```bash
git clone https://github.com/MuhammadWaseem29/BackupFinder.git
cd BackupFinder
go build -o backupfinder ./cmd/backupfinder/
sudo mv backupfinder /usr/local/bin/
```

### Verify Installation

```bash
# Check version
backupfinder version

# Verify assets are embedded  
backupfinder health-check

# Quick test
backupfinder -u https://example.com --silent | head -3
```

---

## Usage

```
Usage:
  backupfinder [flags]

Flags:
INPUT:
  -u, -target string       target URL/domain to scan
  -l, -list string         file containing list of targets

PATTERNS:
  -w, -wordlist            use wordlist mode (comprehensive 1900+ patterns)
  -e, -extensions string   custom extensions file

OUTPUT:
  -o, -output string       file to write output to
      -je string           export to JSON file
      -json                JSON output format
      -silent              show only results
  -v, -verbose             verbose mode

PERFORMANCE:
  -c, -concurrency int     number of concurrent workers (default 10)
      -rate-limit int      rate limit for requests (default 50)
      -timeout int         request timeout in seconds (default 30)
      -retries int         maximum number of retries (default 3)

CONFIGURATION:
      -no-color            disable colored output
      -timestamp           add timestamps to output
      -stats               show statistics (default true)
      -store-resp          store responses
      -store-resp-dir      response storage directory (default "responses")

COMMANDS:
  version                  show version information
  health-check             verify installation and assets
  templates                list available pattern templates
  help                     show this help message
```

---

## Examples

### Basic Usage
```bash
# Basic scan (92 extension patterns)
backupfinder -u https://dev.apple.com

# Comprehensive scan (1907 wordlist patterns)  
backupfinder -u https://dev.apple.com -w

# Multiple targets
backupfinder -l targets.txt
```

### Pattern Generation
```bash
# Generate patterns silently for piping
backupfinder -u https://dev.apple.com --silent
backupfinder -u https://dev.apple.com -w --silent 

# Save to file
backupfinder -u https://dev.apple.com -w -o patterns.txt
```

### Output Formats
```bash
# Verbose mode with statistics
backupfinder -u https://dev.apple.com -w -v

# JSON export
backupfinder -u https://dev.apple.com -w --json -o results.json

# Silent mode (perfect for automation)
backupfinder -u https://dev.apple.com -w --silent
```

### FFUF Integration
```bash
backupfinder -u https://dev.apple.com --silent | ffuf -w /dev/stdin -u https://dev.apple.com/FUZZ -mc 200,403,500 -t 50
ffuf -w patterns.txt -u https://dev.apple.com/FUZZ -mc 200,403,500 -fc 404 -t 50 -o results.txt
```

---

## Integration

### Complete Bug Bounty Workflow
```bash
# Find subdomains
subfinder -d apple.com -silent > subdomains.txt

# Check live targets
cat subdomains.txt | httpx -silent > live_subdomains.txt

# Generate patterns for all subdomains
cat live_subdomains.txt | while read url; do 
    backupfinder -u "$url" --silent >> all_patterns.txt
done

# Scan with ffuf
cat live_subdomains.txt | while read url; do 
    backupfinder -u "$url" --silent | ffuf -w /dev/stdin -u "$url/FUZZ" -mc 200,403,500 -fc 404 -t 50 > results.txt
done
```

### Direct Piping
```bash
backupfinder -u https://dev.apple.com --silent | ffuf -w /dev/stdin -u https://dev.apple.com/FUZZ
backupfinder -u https://dev.apple.com --silent | httpx -status-code
```

### Automation Pipeline
```bash
subfinder -d apple.com -silent | httpx -silent | head -5 | while read url; do 
    backupfinder -u "$url" -w --silent | ffuf -w /dev/stdin -u "$url/FUZZ" -mc 200,403,500 -fc 404 -t 50
done
```

### Multiple Targets
```bash
echo -e "https://dev.apple.com\nhttps://api.apple.com" | while read url; do 
    backupfinder -u "$url" --silent | ffuf -w /dev/stdin -u "$url/FUZZ" -mc 200,403,500 -t 30
done
```

---

## Troubleshooting

### Common Installation Issues

1. **"module contains a go.mod file" error**: Use `@latest` or `@v1.0.0`, NOT `@v2.x.x`
2. **"package not found" error**: Run `go clean -modcache` first
3. **Permission denied**: Use `sudo` for system-wide installation

### Getting Help

- 🐛 **Issues**: [GitHub Issues](https://github.com/MuhammadWaseem29/BackupFinder/issues)
- 📚 **Documentation**: This README
- 💬 **Discussions**: [GitHub Discussions](https://github.com/MuhammadWaseem29/BackupFinder/discussions)

---

## Features

- 9000+ backup patterns in wordlist mode
- Smart subdomain handling (admin.example.com → admin.zip, admin-example.sql)  
- Professional JSON export for automation
- Real-time statistics with performance metrics
- Concurrent processing for fast pattern generation
- Custom wordlists support
- Silent mode for integration with other tools

---

## Pattern Types

### Extension Mode (Default - 92 patterns)
Common backup extensions: `.bak`, `.backup`, `.old`, `.sql`, `.zip`, etc.

### Wordlist Mode (-w - 1900+ patterns)  
Comprehensive patterns for database dumps, configuration backups, archive variants

### Custom Extensions (-e)
Use your own pattern file (one pattern per line, supports `#` comments)

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Author

**MuhammadWaseem**  
GitHub: [@MuhammadWaseem29](https://github.com/MuhammadWaseem29)  
Tool: BackupFinder v1.0.0

---

May you be well on your side of the screen :)

---

## Special Thanks

This project is inspired by and integrates with the following amazing open-source tools:

- [ffuf](https://github.com/ffuf/ffuf.git) — Fast web fuzzer
- [ProjectDiscovery](https://github.com/projectdiscovery) — Security automation tools
  - [subfinder](https://github.com/projectdiscovery/subfinder.git) — Subdomain discovery
  - [httpx](https://github.com/projectdiscovery/httpx.git) — HTTP probing

Thank you to the authors and maintainers of these projects for their contributions to the security community!
