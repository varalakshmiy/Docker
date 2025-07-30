Here is a single **Dockerfile example that shows all three use cases of the `ADD` instruction**:

1. Copying a normal file
2. Automatically extracting a tar file
3. Downloading a file from the internet

After building the image, we’ll run a container and verify that all files are visible live inside the container.

---

## ✅ Folder Structure (Before Build)

Make sure you have the following files in the build context (same folder as the Dockerfile):

```
.
├── Dockerfile
├── note.txt
├── archive.tar.gz         # contains: index.html and style.css
```

You can create `note.txt` like this:

```bash
echo "This is a sample note." > note.txt
```

You can create the tar file:

```bash
mkdir webfiles
echo "<h1>Hello</h1>" > webfiles/index.html
echo "body {color: red;}" > webfiles/style.css
tar -czf archive.tar.gz webfiles
```

---

## 📄 Dockerfile (Complete ADD Use Case)

```Dockerfile
FROM ubuntu:20.04

# Set working directory
WORKDIR /demo

# Update and install tools to view files
RUN apt-get update && apt-get install -y curl

# 1. Copy a normal text file
ADD note.txt /demo/

# 2. Add a tar.gz file (it will be automatically extracted)
ADD archive.tar.gz /demo/

# 3. Download a file from the internet
ADD https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh /demo/wait-for-it.sh

# Give execute permission to downloaded script
RUN chmod +x /demo/wait-for-it.sh

# Final command: List all files and directories
CMD ["bash", "-c", "ls -R /demo && echo 'ADD command demo complete.'"]
```

---

## 🔨 Build the Docker Image

```bash
docker build -t demo-add-usecases .
```

---

## 🚀 Run the Container

```bash
docker run --rm demo-add-usecases
```

**Expected Output:**

```
/demo:
archive.tar.gz
note.txt
wait-for-it.sh
webfiles

/demo/webfiles:
index.html
style.css

ADD command demo complete.
```

---

## 💡 How to Explain This in an Interview

**Question:**
"Can you explain different use cases of the `ADD` instruction and when it's preferred over `COPY`?"

**Answer Structure:**

1. **Basic Copying:** `ADD` behaves like `COPY` for local files (we copied note.txt).
2. **Archive Extraction:** `ADD` can automatically extract `.tar`, `.tar.gz`, and similar archives into the image (we added archive.tar.gz which extracted `index.html` and `style.css`).
3. **Remote URL Support:** `ADD` can download files from the internet directly (we fetched a GitHub-hosted shell script).
4. **Best Practice:** While `ADD` is powerful, `COPY` is preferred for clarity unless archive extraction or remote download is truly needed.

