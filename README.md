# S3/R2 Explorer for VS Code

A powerful VS Code extension for browsing and managing S3-compatible storage, specifically designed to work seamlessly with **Cloudflare R2**, AWS S3, MinIO, and other S3-compatible services.

## ✨ Features

### 🗂️ File Explorer Interface

- **Tree View**: Browse buckets and objects like a local file system
- **Pagination**: Efficient loading with "Load more..." for large buckets
- **Icons & Metadata**: File type icons with size and modification date
- **Search**: Server-side prefix search and client-side content filtering

### 📝 Inline Editing

- **Direct Editing**: Open S3 objects directly in VS Code editors
- **Auto-save**: Changes automatically sync back to S3
- **Text Detection**: Smart handling of text vs binary files
- **Size Limits**: Configurable size limits for large file protection

### 🔄 File Operations

- **Upload**: Drag & drop files or use context menu
- **Download**: Save objects to local filesystem
- **CRUD Operations**: Create folders, rename, delete, copy, move
- **Bulk Operations**: Multi-select support with progress tracking
- **Multipart Uploads**: Efficient handling of large files (>100MB)

### 🔗 Advanced Features

- **Presigned URLs**: Generate temporary shareable links with custom expiry
- **Metadata Viewer**: View complete object metadata and headers
- **Search**: Find objects by prefix or content matching
- **Progress Tracking**: Real-time progress for all operations
- **Error Handling**: Graceful error handling with helpful messages

### 🛡️ Security & Reliability

- **Authentication**: Secure credential storage in VS Code settings
- **Retry Logic**: Automatic retry with exponential backoff
- **Rate Limiting**: Respects S3 API rate limits
- **Caching**: Intelligent caching with TTL for performance
- **Path-style URLs**: Full R2 compatibility

## 🚀 Quick Start

### 1. Install the Extension

Install from the VS Code Marketplace or build from source.

### 2. Configure Your S3/R2 Endpoint

Open VS Code settings (`Ctrl/Cmd + ,`) and configure:

```jsonc
{
  // Required: Your S3-compatible endpoint
  "s3x.endpointUrl": "https://<account>.<jurisdiction>.r2.cloudflarestorage.com",

  // Required: Your access credentials
  "s3x.accessKeyId": "your-access-key-id",
  "s3x.secretAccessKey": "your-secret-access-key",

  // Required for R2: Use path-style URLs
  "s3x.forcePathStyle": true,

  // Optional: Region (R2 works with us-east-1)
  "s3x.region": "us-east-1",

  // Optional: Max file size for direct editing (default: 10MB)
  "s3x.maxPreviewSizeBytes": 10485760,
  
  // Optional: Explicitly allow insecure HTTP endpoints (NOT recommended for production, default: false)
  "s3x.allowHttp": false  
}
```

⚠️ **Warning**:  
The `s3x.allowHttp` option is intended **only for development or test environments on trusted networks**.  
Never enable this option in production, as it allows unencrypted HTTP connections and may expose sensitive data.

### 3. Open the Explorer

- Click the S3/R2 Explorer icon in the Activity Bar
- Or use `Ctrl/Cmd + Shift + P` → "S3: Configure S3/R2 Settings"

## 📊 Cloudflare R2 Setup

### Get Your R2 Credentials

1. **Create R2 API Token**:

   - Go to Cloudflare Dashboard → R2 Object Storage → Manage R2 API tokens
   - Click "Create API token"
   - Select "Custom token" for specific permissions

2. **Find Your Account ID**:

   - Available in your Cloudflare dashboard sidebar
   - Format: `1234567890abcdef1234567890abcdef`

3. **Determine Your Jurisdiction**:
   - Most accounts: `auto` (use `auto.r2.cloudflarestorage.com`)
   - EU accounts: `eu` (use `eu.r2.cloudflarestorage.com`)

### R2 Endpoint Format

```
https://<account-id>.<jurisdiction>.r2.cloudflarestorage.com
```

**Examples**:

- US/Global: `https://abc123.auto.r2.cloudflarestorage.com`
- EU: `https://abc123.eu.r2.cloudflarestorage.com`

### Sample R2 Configuration

```jsonc
{
  "s3x.endpointUrl": "https://abc123def456.auto.r2.cloudflarestorage.com",
  "s3x.region": "us-east-1",
  "s3x.accessKeyId": "your-r2-access-key-id",
  "s3x.secretAccessKey": "your-r2-secret-access-key",
  "s3x.forcePathStyle": true
}
```

## 🎯 Usage Guide

### Basic Operations

#### Browsing

- **Expand buckets** to view contents
- **Click objects** to open them in the editor
- **Use "Load more..."** for pagination in large buckets

#### Uploading

- **Right-click** bucket/folder → "Upload File" or "Upload Folder"
- **Drag & drop** files from your file system into the tree
- **Progress tracking** shows upload status

#### Creating Folders

- **Right-click** bucket/folder → "New Folder"
- Enter folder name (S3 will create it as a prefix)

#### Downloading

- **Right-click** object → "Download"
- Choose save location

#### Searching

- **Command Palette** → "S3: Search in Bucket"
- Choose **prefix search** (server-side, faster) or **contains search** (client-side)

### Advanced Features

#### Presigned URLs

1. Right-click object → "Generate Presigned URL"
2. Choose expiry time (15 minutes to 7 days)
3. URL automatically copied to clipboard

#### Object Metadata

- Right-click object → "Show Object Metadata"
- View size, content type, storage class, custom metadata

#### Bulk Operations

- **Multi-select** objects (Ctrl/Cmd + click)
- Perform bulk delete, copy, or move operations

## ⚙️ Configuration Reference

| Setting                   | Description                                        | Default       | R2 Required |
| ------------------------- | -------------------------------------------------- | ------------- | ----------- |
| `s3x.endpointUrl`         | S3-compatible endpoint URL                         | `""`          | ✅           |
| `s3x.accessKeyId`         | Access Key ID                                      | `""`          | ✅           |
| `s3x.secretAccessKey`     | Secret Access Key                                  | `""`          | ✅           |
| `s3x.forcePathStyle`      | Use path-style URLs                                | `true`        | ✅           |
| `s3x.region`              | AWS region for SigV4                               | `"us-east-1"` | ⚠️          |
| `s3x.maxPreviewSizeBytes` | Max file size for editing                          | `10485760`    | ❌           |
| `s3x.allowHttp`           | Allow insecure HTTP connections (development only) | `false`       | ❌           |

⚠️ **Note**:

- R2 works with any region, but `us-east-1` is recommended.
- `s3x.allowHttp` should only be enabled in trusted **development/test environments**, never in production.

## 🔧 Commands

| Command                        | Description                          | Shortcut |
| ------------------------------ | ------------------------------------ | -------- |
| `S3: Configure S3/R2 Settings` | Open configuration wizard            |          |
| `S3: Search in Bucket`         | Search objects by prefix/content     |          |
| `S3: Refresh`                  | Refresh tree view                    |          |
| `S3: Run Smoke Test`           | Test connection and basic operations |          |

### Context Menu Commands

- **New Folder** - Create a new folder/prefix
- **Upload File** - Upload single or multiple files
- **Upload Folder** - Upload entire directory (recursive)
- **Download** - Download object to local file
- **Rename** - Rename object or folder
- **Copy** - Copy object to another location
- **Move** - Move object to another location
- **Delete** - Delete object or folder
- **Generate Presigned URL** - Create shareable link
- **Show Object Metadata** - View detailed metadata

## 🔒 Security Notes

### Credential Storage

- Credentials are stored in VS Code's settings
- Use **workspace settings** for project-specific credentials
- Use **user settings** for global credentials
- Consider using environment variables in CI/CD

### Best Practices

- **Least Privilege**: Grant minimal required permissions
- **Token Rotation**: Regularly rotate API tokens
- **Endpoint Verification**: Ensure endpoint URLs are correct
- **HTTPS Only**: Extension enforces HTTPS for security
- **Insecure Mode**: If `s3x.allowHttp` is enabled, connections will use plain HTTP. This should only be done on trusted internal networks for development or testing.

## 🛠️ Development

### Building from Source

```bash
# Clone the repository
git clone https://github.com/larsniet/s3x-explorer
cd s3x-explorer

# Install dependencies
npm install

# Compile TypeScript
npm run compile

# Open in VS Code development host
code .
# Press F5 to launch Extension Development Host
```

### Project Structure

```
src/
├── extension.ts          # Main activation & command registration
├── types.ts              # TypeScript interfaces
├── s3/
│   ├── client.ts         # S3 client configuration
│   ├── listing.ts        # Bucket/object listing
│   └── ops.ts            # CRUD operations
├── tree/
│   ├── explorer.ts       # TreeDataProvider
│   └── nodes.ts          # Tree node definitions
├── fs/
│   └── provider.ts       # FileSystemProvider (s3x://)
├── ui/
│   ├── progress.ts       # Progress tracking
│   └── prompts.ts        # User interaction
└── util/
    ├── cache.ts          # In-memory caching
    └── paths.ts          # Path utilities
```

### Testing

#### Smoke Test

Run the built-in smoke test:

1. **Command Palette** → "S3: Run Smoke Test"
2. Tests connection, listing, upload, and cleanup
3. Reports success/failure with detailed logs

#### Manual Testing

1. Configure test R2 bucket
2. Test all CRUD operations
3. Verify multipart uploads with large files
4. Test error scenarios (auth failures, network issues)

## 🐛 Troubleshooting

### Common Issues

#### "Authentication failed"

- **Check credentials**: Verify Access Key ID and Secret
- **Check permissions**: Ensure token has required R2 permissions
- **Check endpoint**: Verify the R2 endpoint URL format

#### "Cannot connect to endpoint"

- **Check URL format**: Must be `https://account.jurisdiction.r2.cloudflarestorage.com`
- **Check network**: Verify internet connectivity
- **Check firewall**: Ensure HTTPS traffic is allowed
- **If using HTTP**:

  * Ensure the endpoint starts with `http://`
  * Set `"s3x.allowHttp": true` in your configuration
  * Only use in trusted development/test networks

#### "Files not opening"

- **Check size limit**: Adjust `maxPreviewSizeBytes` for large files
- **Binary files**: Extension will warn about non-text files
- **Permissions**: Ensure read permissions on objects

#### "Upload failures"

- **File size**: Large files use multipart upload (may take time)
- **Network**: Check for stable internet connection
- **Permissions**: Ensure write permissions on bucket/prefix

### Error Codes

| Error                | Cause                    | Solution                          |
| -------------------- | ------------------------ | --------------------------------- |
| `NoSuchBucket`       | Bucket doesn't exist     | Check bucket name and permissions |
| `NoSuchKey`          | Object doesn't exist     | Refresh view or check path        |
| `Forbidden` (403)    | Insufficient permissions | Check API token permissions       |
| `Unauthorized` (401) | Invalid credentials      | Verify Access Key and Secret      |
| `NetworkingError`    | Connection failure       | Check endpoint URL and network    |

### Debug Mode

Enable VS Code Developer Tools (`Help` → `Toggle Developer Tools`) to see detailed logs in the console.

## 📄 License

MIT License - see LICENSE file for details.

## 🤝 Contributing

Contributions welcome! Please read CONTRIBUTING.md for guidelines.

### Feature Requests

- Object versioning support
- Bucket lifecycle management
- Server-side encryption configuration
- CDN integration for R2

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/larsniet/s3x-explorer/issues)
- **Discussions**: [GitHub Discussions](https://github.com/larsniet/s3x-explorer/discussions)
- **Documentation**: [Wiki](https://github.com/larsniet/s3x-explorer/wiki)

---

**Made with ❤️ for the developer community**

_Tested with Cloudflare R2, AWS S3, MinIO, and other S3-compatible services._
