# wgetjs

**wget for Single Page Applications (SPAs) and JavaScript-heavy websites**

wgetjs is a command-line tool that brings wget-style recursive downloading to modern dynamic websites. Unlike traditional wget, wgetjs uses a headless Chrome browser to render JavaScript, ensuring you capture the fully-rendered HTML that SPAs and dynamic sites generate.

## Why wgetjs?

Traditional tools like `wget` and `curl` only download the initial HTML response. For sites built with React, Vue, Angular, or any framework that renders content client-side, you end up with empty shells. wgetjs solves this by:

- **Rendering JavaScript** - Executes all client-side code before capturing HTML
- **Waiting for dynamic content** - Lets SPAs fully load before saving
- **Recursive crawling** - Follows links through JavaScript-rendered navigation
- **Multiple workers** - Parallel downloads with `-P` for faster mirroring

## Usage

```bash
# Basic recursive download
wgetjs -rc -np http://somesite.com/

# With parallel workers
wgetjs -rc -np -P 4 http://somesite.com/

# Limit recursion depth
wgetjs -rc -l 3 http://somesite.com/
```

## Options

| Option | Description |
|--------|-------------|
| `-rc` | Recursive download (follow links) |
| `-np` | No parent — don't ascend to parent directories |
| `-P N` | Number of parallel workers (default: 1) |
| `-l N` | Max recursion depth (default: 5) |
| `-w SECONDS` | Wait SECONDS between retrievals |
| `--waitretry=SECONDS` | Wait 1..SECONDS between retries (backoff) |
| `--random-wait` | Wait 0.5×WAIT to 1.5×WAIT secs (randomized) |
| `--method=CMD` | Custom command to fetch HTML (alternative to Chrome) |
| `-h, --help` | Show help |

## Custom Fetch Method

By default, wgetjs uses headless Chrome via the Chrome DevTools Protocol. If you prefer a different renderer, use `--method` with a command that outputs HTML to stdout:

```bash
# Example with lightpanda
wgetjs --method="lightpanda fetch --dump" http://somesite.com/
```

## Requirements

- Node.js
- Google Chrome (or Chromium) installed and in PATH
