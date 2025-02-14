# Cleanarr
Intelligent Media Library Manager


## System Architecture

### Core Components

1. **File System Scanner**
   - Recursive directory monitoring
   - Real-time file system events handling
   - Efficient caching of scan results
   - Support for multiple watch directories
   - Network share handling (SMB/NFS)

2. **Metadata Engine**
   ```
   MetadataEngine
   ├── Extractors
   │   ├── FFmpeg Wrapper
   │   ├── MediaInfo Integration
   │   └── Custom Stream Analyzer
   ├── Online Sources
   │   ├── TMDb Client
   │   ├── IMDb Client
   │   └── TVDb Client
   └── Metadata Store
       ├── Local Cache
       └── Database
   ```

3. **Cleaning Pipeline**
   ```
   CleaningPipeline
   ├── Language Processor
   │   ├── Audio Stream Analyzer
   │   └── Subtitle Analyzer
   ├── Naming Processor
   │   ├── Scene Tag Cleaner
   │   └── Title Normalizer
   └── Quality Processor
       ├── Resolution Validator
       └── Bitrate Analyzer
   ```

### Integration Points

1. **External Services**
   - Plex API Integration
   - Emby/Jellyfin Support
   - Sonarr/Radarr API
   - TMDb/IMDb/TVDb APIs

2. **Storage Systems**
   - Local File System
   - NAS Integration (SMB/NFS)
   - Cloud Storage Support
   - Docker Volume Management

## Technical Stack

### Backend
- Python 3.11+
- FastAPI for REST API
- SQLAlchemy for ORM
- FFmpeg-python for media analysis
- aiofiles for async I/O
- PyMediaInfo for detailed media info

### Frontend
- React with TypeScript
- TailwindCSS for styling
- React Query for API state
- Recharts for analytics
- Monaco Editor for advanced config

### Database
- PostgreSQL for primary storage
- Redis for caching
- SQLite as fallback for simple installations

### Container Infrastructure
- Docker support
- Docker Compose for easy deployment
- Health checks and auto-healing
- Volume management for media

## API Design

### RESTful Endpoints

```
/api/v1
├── /scan
│   ├── POST /directory
│   └── GET /status
├── /clean
│   ├── POST /auto
│   └── POST /manual
├── /metadata
│   ├── GET /{media_id}
│   └── PATCH /{media_id}
└── /config
    ├── GET /profiles
    └── PUT /profiles
```

### WebSocket Events

```
ws://cleanarr/events
├── scan_progress
├── clean_progress
└── system_alerts
```

## Configuration Management

### User Preferences
```yaml
cleaning_profiles:
  default:
    audio_languages: ["eng", "jpn"]
    subtitle_languages: ["eng", "spa"]
    remove_tags: true
    clean_metadata: true
    naming_template: "{title} ({year}) {quality}"
```

### System Settings
```yaml
system:
  scan_interval: 3600
  parallel_tasks: 4
  notification:
    discord: true
    email: false
```

## Storage Management

### Directory Structure
```
/media
├── movies
│   └── .cleanarr_cache
└── tv
    └── .cleanarr_cache
```

## Security Considerations

1. **File System**
   - Read-only analysis mode
   - Backup before modifications
   - Checksums for verification

2. **API Security**
   - JWT authentication
   - Rate limiting
   - API key management

3. **Network**
   - HTTPS enforcement
   - Secure WebSocket
   - Local network restriction

## Deployment Options

1. **Docker**
```yaml
version: "3.8"
services:
  cleanarr:
    image: cleanarr:latest
    volumes:
      - /media:/media
      - ./config:/config
    ports:
      - "8080:8080"
    environment:
      - PUID=1000
      - PGID=1000
```

2. **Standalone**
   - Python virtual environment
   - Systemd service
   - Windows service

## Future Expansion

1. **Machine Learning Integration**
   - Automatic language detection
   - Content quality scoring
   - Smart naming suggestions

2. **Advanced Analytics**
   - Library health scores
   - Storage optimization suggestions
   - Quality improvement recommendations

3. **Automation**
   - Scheduled cleaning tasks
   - Conditional rules engine
   - Custom script triggers
