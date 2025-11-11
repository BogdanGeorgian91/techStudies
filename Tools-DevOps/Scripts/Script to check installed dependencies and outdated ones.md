ZSH with unused.tsx
```
#!/bin/zsh

echo "Starting dependency check..."

# Save installed dependencies to a file
echo "Listing installed dependencies..."
npm list --depth=0 > installed.txt
echo "Installed dependencies saved to installed.txt."

# Save outdated dependencies to a file
echo "Checking for outdated dependencies..."
npm outdated > outdated.txt
echo "Outdated dependencies saved to outdated.txt."

# Check for unused dependencies and save to a file
echo "Looking for potentially unused dependencies..."
> unused.txt  # Clear or create the file

for dep in $(npm list --depth=0 | grep '|--' | cut -d' ' -f2 | cut -d@ -f1); do
  if ! find . -type f \( -name "*.js" -o -name "*.tsx" -o -name "*.ts" \) -exec grep -l "$dep" {} + > /dev/null 2>&1; then
    echo "$dep" >> unused.txt
  fi
done

echo "Unused dependencies saved to unused.txt."

echo "Done! Check installed.txt, outdated.txt, and unused.txt for results."
```


ZSH without unused
```
#!/bin/zsh

echo "Starting manual update..."

# List all dependencies
echo "Installed dependencies:"
npm list --depth=0 > deps.txt
cat deps.txt

# Check for outdated ones
echo "\nChecking for outdated dependencies..."
npm outdated > outdated.txt
cat outdated.txt

# Improve unused dependency check
echo "\nLooking for potentially unused dependencies..."
for dep in $(npm list --depth=0 | grep '|--' | cut -d' ' -f2 | cut -d@ -f1); do
  if ! find . -type f \( -name "*.js" -o -name "*.tsx" -o -name "*.ts" \) -exec grep -l "$dep" {} + > /dev/null 2>&1; then
    echo "$dep might be unused"
  fi
done

echo "Done! Check deps.txt and outdated.txt manually."
```

BASH
```
#!/bin/bash

echo "Starting manual update..."

# List all dependencies
echo "Installed dependencies:"
npm list --depth=0 > deps.txt
cat deps.txt

# Check for outdated ones
echo -e "\nChecking for outdated dependencies..."
npm outdated > outdated.txt
cat outdated.txt

# Improve unused dependency check
echo -e "\nLooking for potentially unused dependencies..."
for dep in $(npm list --depth=0 | grep '|--' | cut -d' ' -f2 | cut -d@ -f1); do
  if ! find .type f -name "*.js -o -name "*tsx" -o -name "*.ts" | xargs grep -l "$dep" > /dev/null 2>&1; then
    echo "$dep might be unused"
  fi
done

echo "Done! Check deps.txt and outdated.txt manually."
 
# This script will list all dependencies, check for outdated ones, and look for potentially unused dependencies. 

```