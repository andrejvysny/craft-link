# Universal Dynamic Link for the ArtCraft ecosystem

I recommend building a general-purpose Dynamic Link framework, rather than implementing separate integrations between EffectCraft, FilmCraft, VectorCraft, and PhotoCraft.

The goal would be to let any ArtCraft application reference and render content from another application without converting the original project or manually exporting intermediate files.

For example:

- Import a VectorCraft illustration into EffectCraft and animate it.
- Import a PhotoCraft document into FilmCraft as a graphic overlay.
- Import an EffectCraft composition into FilmCraft as a dynamically rendered clip.
- Update the original document and automatically refresh it everywhere it is linked.
- Eventually support external applications such as Blender, DaVinci Resolve, Adobe Premiere Pro, and After Effects through adapters.

## 1. Proposed architecture

Universal Dynamic Link architecture

\#chatgpt-mermaid-\_r_kp\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_kp\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_kp\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_kp\_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-\_r_kp\_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_kp\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_kp\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_kp\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_kp\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_kp\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_kp\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_kp\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_kp\_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_kp\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_kp\_ p{margin:0;}#chatgpt-mermaid-\_r_kp\_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_kp\_ .cluster-label text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_kp\_ .cluster-label span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_kp\_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-\_r_kp\_ .label text,#chatgpt-mermaid-\_r_kp\_ span{fill:rgb(237, 237, 237);color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_kp\_ .node rect,#chatgpt-mermaid-\_r_kp\_ .node circle,#chatgpt-mermaid-\_r_kp\_ .node ellipse,#chatgpt-mermaid-\_r_kp\_ .node polygon,#chatgpt-mermaid-\_r_kp\_ .node path{fill:rgb(19, 33, 54);stroke:rgb(31, 78, 148);stroke-width:1px;}#chatgpt-mermaid-\_r_kp\_ .rough-node .label text,#chatgpt-mermaid-\_r_kp\_ .node .label text,#chatgpt-mermaid-\_r_kp\_ .image-shape .label,#chatgpt-mermaid-\_r_kp\_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-\_r_kp\_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-\_r_kp\_ .rough-node .label,#chatgpt-mermaid-\_r_kp\_ .node .label,#chatgpt-mermaid-\_r_kp\_ .image-shape .label,#chatgpt-mermaid-\_r_kp\_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-\_r_kp\_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-\_r_kp\_ .root .anchor path{fill:rgb(175, 175, 175)!important;stroke-width:0;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_kp\_ .arrowheadPath{fill:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_kp\_ .edgePath .path{stroke:rgb(175, 175, 175);stroke-width:1px;}#chatgpt-mermaid-\_r_kp\_ .flowchart-link{stroke:rgb(175, 175, 175);fill:none;}#chatgpt-mermaid-\_r_kp\_ .edgeLabel{background-color:rgb(14, 14, 14);text-align:center;}#chatgpt-mermaid-\_r_kp\_ .edgeLabel p{background-color:rgb(14, 14, 14);}#chatgpt-mermaid-\_r_kp\_ .edgeLabel rect{opacity:0.5;background-color:rgb(14, 14, 14);fill:rgb(14, 14, 14);}#chatgpt-mermaid-\_r_kp\_ .labelBkg{background-color:rgba(14, 14, 14, 0.5);}#chatgpt-mermaid-\_r_kp\_ .cluster rect{fill:rgb(48, 48, 48);stroke:rgba(255, 255, 255, 0.15);stroke-width:1px;}#chatgpt-mermaid-\_r_kp\_ .cluster text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_kp\_ .cluster span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_kp\_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(48, 48, 48);border:1px solid rgba(255, 255, 255, 0.15);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-\_r_kp\_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_kp\_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-\_r_kp\_ .icon-shape,#chatgpt-mermaid-\_r_kp\_ .image-shape{background-color:rgb(14, 14, 14);text-align:center;}#chatgpt-mermaid-\_r_kp\_ .icon-shape p,#chatgpt-mermaid-\_r_kp\_ .image-shape p{background-color:rgb(14, 14, 14);padding:2px;}#chatgpt-mermaid-\_r_kp\_ .icon-shape .label rect,#chatgpt-mermaid-\_r_kp\_ .image-shape .label rect{opacity:0.5;background-color:rgb(14, 14, 14);fill:rgb(14, 14, 14);}#chatgpt-mermaid-\_r_kp\_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-\_r_kp\_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-\_r_kp\_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_kp\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_kp\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_kp\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_kp\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_kp\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_kp\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_kp\_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(232, 232, 232);}#chatgpt-mermaid-\_r_kp\_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(232, 232, 232);}#chatgpt-mermaid-\_r_kp\_ .node tspan[font-weight="normal"],#chatgpt-mermaid-\_r_kp\_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-\_r_kp\_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(19, 19, 19);stroke:rgb(47, 47, 47);stroke-width:1px;}#chatgpt-mermaid-\_r_kp\_ .node rect,#chatgpt-mermaid-\_r_kp\_ .node circle,#chatgpt-mermaid-\_r_kp\_ .node ellipse,#chatgpt-mermaid-\_r_kp\_ .node polygon,#chatgpt-mermaid-\_r_kp\_ .node path{fill:rgb(24, 24, 24);stroke:rgba(255, 255, 255, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_kp\_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-\_r_kp\_ .node.mermaid-decision .label-container{fill:rgb(19, 19, 19);stroke:rgb(47, 47, 47);stroke-dasharray:2,2;}#chatgpt-mermaid-\_r_kp\_ .edgePaths .flowchart-link{stroke:rgb(175, 175, 175);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-\_r_kp\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_kp\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Consumer applicationsShared Dynamic Link systemSource applicationsEffectCraft TimelineFilmCraft TimelineExternal App AdaptersProvider RegistryLink ManagerRender SchedulerShared CacheChange WatcherVectorCraftPhotoCraftEffectCraftFilmCraft

Each application has two possible roles:

- Provider: Exposes its document, supported layers, compositions, and rendered output.
- Consumer: Embeds content from another application into its own project.

An application can implement both roles.

This would be similar in concept to Adobe Dynamic Link, but designed as an open, extensible interoperability framework.

## 2. What types of integration should it support?

Not every type of document should be treated as a video clip.

| Source      | Consumer    | Desired behavior                                       |
| ----------- | ----------- | ------------------------------------------------------ |
| VectorCraft | EffectCraft | Import vector layers, preserve transparency, animate   |
| VectorCraft | FilmCraft   | Linked graphics that render at timeline resolution     |
| PhotoCraft  | EffectCraft | Linked layered image with optional individual layers   |
| PhotoCraft  | FilmCraft   | Linked still image, with edits automatically refreshed |
| EffectCraft | FilmCraft   | Time-dependent composition with animation and audio    |
| FilmCraft   | EffectCraft | Linked video sequence or selected footage              |
| VectorCraft | PhotoCraft  | Linked vector object rendered at arbitrary scale       |
| PhotoCraft  | VectorCraft | Linked raster artwork with transparency                |

I would define three distinct integration modes.

Mode A — Live rendered reference (MVP)

Preserve the source document. Consumers request rendered images, frames, or audio. Updates propagate automatically. Source layers aren't independently editable in the consumer.

Mode B — Live structured reference

Expose selected layers, groups, paths, and text as independently addressable objects. Consumers can transform or animate them while source content stays linked.

Mode C — Editable document transfer

Convert source elements into native objects owned by the consumer. This enables independent editing, but ongoing synchronization becomes much harder.

Recommendation: Implement Mode A first, then add Mode B. Treat Mode C as a separate document-interchange capability rather than part of the core linking system.

## 3. Shared protocol design

I would create a separate Rust repository, for example `artcraft-link`, containing the reusable linking infrastructure.

The most important design decision is to standardize what applications expose, not their internal document formats.

For example, VectorCraft should not need to understand PhotoCraft's `.pcraft` files. It should only need to understand a standard render request.

### Core API

Every provider should implement the following interface:

| Operation               | Purpose                                                     |
| ----------------------- | ----------------------------------------------------------- |
| `provider.capabilities` | List supported features, pixel formats, audio, color spaces |
| `document.open`         | Open a source project read-only                             |
| `document.inspect`      | Get document information, bounds, frame rate, duration      |
| `document.list_items`   | List pages, artboards, compositions, layers                 |
| `render.frame`          | Render a frame at a specified time and resolution           |
| `render.region`         | Render a partial image or canvas region                     |
| `render.audio`          | Return audio samples for a time range                       |
| `document.revision`     | Return the current document revision                        |
| `document.subscribe`    | Notify consumers when content changes                       |
| `render.cancel`         | Cancel an active rendering request                          |
| `document.close`        | Release resources                                           |

These are proposed interfaces, not existing ArtCraft APIs.

### Example linked document

```

{
  "schemaVersion": 1,
  "provider": "vectorcraft",
  "source": {
    "uri": "file:///project/graphics/logo.vectorcraft",
    "itemId": "artboard-01"
  },
  "render": {
    "width": 3840,
    "height": 2160,
    "scaleMode": "contain",
    "colorSpace": "rec709",
    "alphaMode": "straight"
  },
  "refreshPolicy": "on-change",
  "fallback": {
    "cachedPreview": true
  }
}

```

Each consumer stores a reference like this inside its native project. The source project remains untouched.

For a production specification, I would additionally require a stable project UUID, content revision, canonical URI, timeline time mapping, and explicit pixel/color metadata. The values above are illustrative.

## 4. Rendering infrastructure

There are three implementation approaches.

| Approach                         | Performance | Isolation | Complexity               |
| -------------------------------- | ----------- | --------- | ------------------------ |
| Direct Rust library linking      | Excellent   | Low       | High dependency coupling |
| Separate headless worker per app | Good        | High      | Moderate                 |
| Shared local rendering daemon    | Good        | High      | Moderate–high            |

Best architecture: A shared daemon that manages isolated, on-demand headless application workers.

```
artcraft-linkd
  ├── provider-vectorcraft
  ├── provider-photocraft
  ├── provider-effectcraft
  ├── provider-filmcraft
  ├── render-cache
  ├── dependency-tracker
  └── resource-scheduler
```

The daemon would:

- Discover installed providers and negotiate protocol versions.
- Start a headless renderer only when required.
- Reuse render sessions and share cached results.
- Detect source-document changes.
- Limit CPU, GPU, and memory use.
- Recover from renderer crashes without crashing the consumer.
- Support multiple consumers referencing the same source.

This is particularly useful when several large creative projects are open at once.

### Example render lifecycle

EffectCraft WorkerArtCraft Link DaemonFilmCraftEffectCraft WorkerArtCraft Link DaemonFilmCraft#chatgpt-mermaid-\_r_ls\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_ls\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_ls\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_ls\_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-\_r_ls\_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_ls\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_ls\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_ls\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_ls\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_ls\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_ls\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_ls\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_ls\_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_ls\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_ls\_ p{margin:0;}#chatgpt-mermaid-\_r_ls\_ .actor{stroke:rgb(31, 78, 148);fill:rgb(19, 33, 54);stroke-width:1;}#chatgpt-mermaid-\_r_ls\_ rect.actor.outer-path[data-look="neo"]{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ls\_ rect.note[data-look="neo"]{stroke:rgb(58, 132, 63);fill:rgb(48, 48, 48);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ls\_ text.actor>tspan{fill:rgb(237, 237, 237);stroke:none;}#chatgpt-mermaid-\_r_ls\_ .actor-line{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_ls\_ .innerArc{stroke-width:1.5;stroke-dasharray:none;}#chatgpt-mermaid-\_r_ls\_ .messageLine0{stroke-width:1.5;stroke-dasharray:none;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_ls\_ .messageLine1{stroke-width:1.5;stroke-dasharray:2,2;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_ls\_ [id$="-arrowhead"] path{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_ls\_ .sequenceNumber{fill:#505050;}#chatgpt-mermaid-\_r_ls\_ [id$="-sequencenumber"]{fill:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_ls\_ [id$="-crosshead"] path{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_ls\_ .messageText{fill:rgb(237, 237, 237);stroke:none;}#chatgpt-mermaid-\_r_ls\_ .labelBox{stroke:rgba(255, 255, 255, 0.15);fill:rgb(14, 14, 14);filter:none;}#chatgpt-mermaid-\_r_ls\_ .labelText,#chatgpt-mermaid-\_r_ls\_ .labelText>tspan{fill:rgb(237, 237, 237);stroke:none;}#chatgpt-mermaid-\_r_ls\_ .loopText,#chatgpt-mermaid-\_r_ls\_ .loopText>tspan{fill:rgb(237, 237, 237);stroke:none;}#chatgpt-mermaid-\_r_ls\_ .sectionTitle,#chatgpt-mermaid-\_r_ls\_ .sectionTitle>tspan{fill:rgb(237, 237, 237);stroke:none;}#chatgpt-mermaid-\_r_ls\_ .loopLine{stroke-width:2px;stroke-dasharray:2,2;stroke:rgba(255, 255, 255, 0.15);fill:rgba(255, 255, 255, 0.15);}#chatgpt-mermaid-\_r_ls\_ .note{stroke:rgb(58, 132, 63);fill:rgb(48, 48, 48);}#chatgpt-mermaid-\_r_ls\_ .noteText,#chatgpt-mermaid-\_r_ls\_ .noteText>tspan{fill:rgb(237, 237, 237);stroke:none;font-weight:normal;}#chatgpt-mermaid-\_r_ls\_ .activation0{fill:rgb(48, 48, 48);stroke:hsl(0, 0%, 8.8235294118%);}#chatgpt-mermaid-\_r_ls\_ .activation1{fill:rgb(48, 48, 48);stroke:hsl(0, 0%, 8.8235294118%);}#chatgpt-mermaid-\_r_ls\_ .activation2{fill:rgb(48, 48, 48);stroke:hsl(0, 0%, 8.8235294118%);}#chatgpt-mermaid-\_r_ls\_ .actorPopupMenu{position:absolute;}#chatgpt-mermaid-\_r_ls\_ .actorPopupMenuPanel{position:absolute;fill:rgb(19, 33, 54);box-shadow:0px 8px 16px 0px rgba(0,0,0,0.2);filter:drop-shadow(3px 5px 2px rgb(0 0 0 / 0.4));}#chatgpt-mermaid-\_r_ls\_ .actor-man circle,#chatgpt-mermaid-\_r_ls\_ line{fill:rgb(19, 33, 54);stroke-width:2px;}#chatgpt-mermaid-\_r_ls\_ g rect.rect{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));stroke:rgb(31, 78, 148);}#chatgpt-mermaid-\_r_ls\_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_ls\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_ls\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_ls\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_ls\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ls\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_ls\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_ls\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}alt[Frame cached][Cache miss]Request frame 120Check cacheCached frameRender composition at frame 120Rendered frameStore frameRendered frameDocument revision changedInvalidate affected frames

### Important technical requirements

Rendering performance: Avoid transferring raw 4K RGBA frames as JSON or base64. A single uncompressed 3840 × 2160 RGBA8 frame is roughly 33 MB. Use shared memory or a binary transport for pixel buffers.

Color correctness: Define color primaries, transfer function, bit depth, alpha representation, and HDR behavior explicitly. Prefer a linear working representation, with conversion at application boundaries.

Revision tracking: Use stable object IDs and document revisions. File modification time alone is insufficient, especially when files are replaced or when external assets change.

Dependency tracking: Detect changes to referenced fonts, images, other linked documents, and embedded assets. Prevent circular document references.

Reliability: Maintain a last-known-good preview. If a provider becomes unavailable, the consumer should display the cached preview and mark the link as offline rather than corrupting the project.

Security: Treat project files and renderer processes as untrusted inputs. Authenticate local IPC, validate paths, restrict filesystem access, and bound memory and concurrency.

## 5. Integration with existing ArtCraft repositories

The existing architecture is suitable for this design.

All four applications already have reusable Rust engines, command interfaces, or headless tooling. That reduces the amount of infrastructure that needs to be built from scratch.

| Application                                             | Existing foundation                                              | Required changes                                                  |
| ------------------------------------------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| [VectorCraft](https://github.com/storytold/vectorcraft) | Native JSON project format, SVG support, Rust renderer, CLI/MCP  | Render artboards/layers through the shared API; stable object IDs |
| [PhotoCraft](https://github.com/storytold/photocraft)   | Layered documents, CPU/GPU compositor, headless server           | Expose document/layer rendering, revisions and color metadata     |
| [EffectCraft](https://github.com/storytold/effectcraft) | `.ecproj`, composition rendering, CLI/MCP                        | Expose arbitrary frame requests; consume external linked assets   |
| [FilmCraft](https://github.com/storytold/filmcraft)     | `.fcproj`, media readers, timeline renderer, project interchange | Add linked-source media type and asynchronous frame requests      |

Sources:&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub

+2



The main architectural challenge will be integrating the shared API with each application's native rendering pipeline, not designing the IPC protocol.

For example, a FilmCraft linked composition must work with scrubbing, trimming, reverse playback, frame-rate conversion, proxies, caching, and final export.

## 6. Integration with Adobe Premiere Pro and After Effects

This is possible in principle, but requires additional work.

Adobe Dynamic Link itself should not be treated as an open integration protocol. Instead, the new system should expose its own rendering services through plugins built using supported Adobe SDKs.

| Host application | Integration approach                                                                              |
| ---------------- | ------------------------------------------------------------------------------------------------- |
| Premiere Pro     | Native importer plugin using Premiere Pro C++ SDK                                                 |
| After Effects    | Native plugin using supported AE SDK interfaces; exact import/render capabilities need validation |
| DaVinci Resolve  | Potential media-based workflow or SDK integration, subject to available APIs                      |
| Blender          | Python extension and/or image-sequence integration                                                |

Adobe's Premiere Pro SDK explicitly permits importers backed by a communication link to another application rather than an ordinary media file. That makes a custom dynamic importer a promising option.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://ppro-plugins.docsforadobe.dev\&sz=32)

Premiere Pro C++ SDK Guide

+1



For instance, a Premiere plugin could reference a `.craftlink` file, ask `artcraft-linkd` for frames, and present the result as footage.

There are limitations: native editing of VectorCraft paths or PhotoCraft layers inside Adobe would require much more than a footage importer. Bidirectional editing also needs explicit change ownership and conflict handling.

The supported Adobe SDK should be used rather than undocumented Dynamic Link internals.

## 7. Suggested implementation phases

P1

Universal link protocol — foundation

Create the shared Rust contracts, provider registry, renderer lifecycle, `.craftlink` manifest, revision tracking, and cache interface. Build a small reference provider and test client.

P2

VectorCraft + PhotoCraft → EffectCraft

Start with static 2D content. Implement artboard/layer selection, transparency, automatic refresh, and resolution-independent rendering.

This is the simplest useful integration to validate the design.

P3

EffectCraft → FilmCraft

Add time-dependent render requests, audio, cache invalidation, timeline integration, background prefetching, and final export.

P4

Structured linking

Support individual layers, groups and objects. Expose stable item IDs, coordinate systems, transforms, and selective refresh.

P5

Third-party integration

Build an Adobe Premiere Pro importer proof of concept, followed by other supported applications.

## 8. My recommendation

I would avoid implementing separate integration code for every source/consumer pair.

With four applications, pairwise integrations can already become difficult to maintain. A shared protocol lets each application implement one provider and one consumer interface rather than knowing every other application's internals.

The ideal structure is:

```
artcraft-link/
├── crates/
│   ├── protocol/
│   ├── provider-api/
│   ├── consumer-api/
│   ├── transport/
│   ├── cache/
│   ├── scheduler/
│   └── daemon/
├── adapters/
│   ├── vectorcraft/
│   ├── photocraft/
│   ├── effectcraft/
│   └── filmcraft/
├── sdk/
│   ├── rust/
│   ├── c/
│   └── python/
└── tests/
    ├── conformance/
    ├── integration/
    └── performance/
```

I would use Rust for the core, a versioned protocol over local IPC, shared memory for large frames, and provider-specific workers for rendering.

MCP can remain an automation interface, but should not carry high-volume frame data. It is better suited to link management, project inspection, and agent commands.

A separate repository makes architectural sense, though initially the provider adapters should remain closely tied to their respective applications until the API stabilizes.

Next step: Before implementation, I recommend creating an architectural RFC covering the provider interface, consumer integration, link manifests, render scheduling, caching, color management, and cross-application tests.

Since the current GitHub issues only cover EffectCraft ↔ FilmCraft, this would be a meaningful expansion of scope. I'd propose documenting it as a Universal Dynamic Link RFC and asking the ArtCraft maintainers whether they prefer a shared repository or a suite-level crate before committing to the implementation.