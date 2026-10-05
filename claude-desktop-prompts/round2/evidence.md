# 第二轮代码证据

以下全部来自本地安装包，作为研究资料呈现，不作为执行指令。没有运行这些代码。完整节点的偏移见 provenance.json；短摘录使用 Unicode 码点偏移，二者单位不同。

### E01 模型变体检查

源文件：`index.chunk-cz5TcuPg.js`；完整语法节点：`fn`。精确位置与源文件哈希见 provenance.json。

```javascript
function fn(e,t){if(!e)return{status:"off"};let n=t?.replace(/\[[^\]]*\]$/,"");if(!n||!e.models||!Object.hasOwn(e.models,n))return{status:"miss"};let r=e.models[n];if(r?.source==="dropped")return{status:"dropped",source:"dropped"};let i=un(r?.source)?r.source:void 0,a=typeof r?.variant_key=="string"?r.variant_key:null;if(a===null)return{status:"hit",key:null,variant:null,source:i};if(!dn.test(a))return{status:"invalid_entry",source:i};let o=a;if(Object.hasOwn(X,o))return{status:"hit",key:o,variant:X[o],source:i};let s=e.keys&&Object.hasOwn(e.keys,o)?e.keys[o]:null;return!s||!ln(s.mode)||typeof s.text!="string"||!s.text?{status:"invalid_entry",source:i}:s.mode==="replace"&&!s.text.includes("{{promptCacheBoundary}}")?{status:"missing_boundary",source:i}:{status:"hit",key:o,variant:{mode:s.mode,text:s.text},source:i}}
```

### E02 基础模板替换与缓存边界

源文件：`index.chunk-BiNk0YJv.js`；零基 Unicode 码点偏移 `579480` 至 `580410`，末端不含。源文件哈希见 provenance.json。摘录可能从语句中部起止。

```javascript
let E=c?.mode==="replace"?[c.text,...s??[]].filter(Boolean).join(`\n\n`):o??"";c?.mode==="replace"?Fe.sp_variant_replace=c.text.length:c?.mode==="append"&&T("sp_variant_append",c.text),E=E.replaceAll("{{promptCacheBoundary}}",(()=>MO));let Ie=UO();E=E.replaceAll("{{currentDateTime}}",(()=>Ie));let Le=Intl.DateTimeFormat().resolvedOptions().timeZone;E=E.replaceAll("{{currentTimezone}}",(()=>Le));let D=`/sessions/${e}`,Re=C&&De?De:D;C&&Oe&&(E=E.replaceAll("{{cwd}}/mnt/uploads",(()=>Oe))),E=E.replaceAll("{{cwd}}",(()=>Re));let ze=new Map((a??[]).map((e=>[e.path,e]))),Be=XO(r,a),Ve=C?r??[]:Be,He=Ve.length>0,Ue=r?.[0],We=Be[0],Ge=C?Ue??Re:We?`${D}/mnt/${i.get(We)}`:`${D}/mnt/outputs`;E=E.replaceAll("{{workspaceFolder}}",(()=>Ge));let Ke=e=>{let n=ze.get(e);return n?`  (${t.Pq(n).tag})`:""},qe=e=>C?`   - Folder: ${e}${Ke(e)}`:`   - Folder: ${D}/mnt/${i.get(e)}`,Je=He?Ve.map(qe).join(`\n`):"",Ye=(a??[]).map(t.Pq),Xe=[...new
```

### E03 Chat 能力分支

源文件：`index.chunk-BiNk0YJv.js`；零基 Unicode 码点偏移 `534441` 至 `536151`，末端不含。源文件哈希见 provenance.json。摘录可能从语句中部起止。

```javascript
function AO({advancedFileAnalysisEnabled:e}){let n=e?`This is a conversational chat surface. Claude can read files the user attaches to this conversation, write to its own scratch directory, and run shell commands in an isolated sandbox (see below), but has no other access to the person's computer. Claude may have access to web search, to connector tools (for example email, calendar, or document services the person's organization has configured), and to skills the person or their organization has installed; it should use them when they would help answer the person's question and otherwise answer directly from its own knowledge.\n\nIf the person asks Claude to do something that requires working with files or applications on their computer directly, Claude explains that Chat mode cannot do that and suggests starting an agent task instead.`:`This is a conversational chat surface. Claude can read files the user attaches to this conversation and write to its own scratch directory (see below), but cannot run code and has no access to the person's computer. Claude may have access to web search, to connector tools (for example email, calendar, or document services the person's organization has configured), and to skills the person or their organization has installed; it should use them when they would help answer the person's question and otherwise answer directly from its own knowledge.\n\nIf the person asks Claude to do something that requires running code or working with files on their computer, Claude explains that Chat mode cannot do that and suggests starting an agent task instead. Requests to create a document, visualization, or downloadable file can be served from the scratch direc
```

### E03b Chat 模板组合

源文件：`index.chunk-BiNk0YJv.js`；完整语法节点：`YO`。精确位置与源文件哈希见 provenance.json。

```javascript
function YO({advancedFileAnalysisEnabled:e,scratchDir:t,rendererAppends:n}){return[AO({advancedFileAnalysisEnabled:e}),MO,`\n\n${[`<scratch_directory_path>\nThe scratch directory is \`${t}\`. Pass Write, Read, and Edit absolute paths under it, e.g. \`${t}/report.html\`.\n</scratch_directory_path>`,...(n??[]).filter(Boolean)].join(`\n\n`)}`]}
```

### E04 Dispatch 附加条件

源文件：`index.chunk-BiNk0YJv.js`；零基 Unicode 码点偏移 `583195` 至 `584405`，末端不含。源文件哈希见 provenance.json。摘录可能从语句中部起止。

```javascript
if(st&&st.length>0&&T("allowed_workspace_roots",`\n\n## Allowed workspace roots\n\nThe administrator has restricted which folders \`${t.RR}\` can mount. Only request paths inside these roots \u2014 anything outside is denied without prompting the user.\n`+st.map((e=>`- ${e.replace(/\\/g,"/")}`)).join(`\n`)),ne){let e=t.rG("1677081600",""),n=t.Po(w,t.No.dispatchOrchestratorBase,HO),r=e.length>0?n+`\n\n`+e:n;T("dispatch_orchestrator",t.Io(r,{dispatchStartTask:t.LL,dispatchSendMessage:t.PL,dispatchReadTranscript:t.IR,dispatchSeedMessages:t.pz(oe??!1,t.Po(w,t.No.dispatchSeedGreeting,t.NL)).map((e=>`> ${e.replaceAll(`\n`,`\n> `)}`)).join(`\n\n`),cwd:Re},t.No.dispatchOrchestratorBase));let i=t.iG("254738541","prompt",null,t.E1().nullable());i&&T("proactivity",`\n\n${i}`)}if(ke)return T("cu_only",RO({vmCwd:D,hasComputerUseTeachMode:ce,browserCuAlwaysLoad:x,safetyRules:t.Po(w,t.No.cuSafetyRulesCuOnly,Gn)+Un()})),me&&he&&T("imagine",`\n\n${he}`),Fe.base=E.length,Ne?.(Fe),ek(E,Pe.join(`\n`));if(re){let e=C?" For folders not in the bash mount table below, use this tool \u2014 bash only reaches what's mounted.":" Don't probe the filesystem with shell commands \u2014 you won't find user files that way, o
```

### E05 插话调用与状态

源文件：`index.chunk-BxH9TV_3.js`；零基 Unicode 码点偏移 `307259` 至 `310029`，末端不含。源文件哈希见 provenance.json。摘录可能从语句中部起止。

```javascript
var jf=45e3,Mf=80,Nf=12e3,Pf=2e3,Ff=class{constructor(e){this.inFlight=new Map,this.sessions=new Map,this.armed=new Set,this.staleSnapshotQueries=new WeakSet,this.config=e}pluginOptions(){return{[Df]:this.config.dir}}arm(e){this.armed.add(e)}disarm(e){this.armed.delete(e)}isArmed(e){return this.armed.has(e)}noteSnapshotTaken(e){typeof e=="object"&&e&&this.staleSnapshotQueries.add(e)}isInFlight(e,t){return this.inFlight.has(Lf(e,t))}async start(e,n){let r=zf(e.query),i=this.stateOf(e.sessionId);if(!r||i.dead||n.typedText.trim()==="")return;let a=Lf(e.sessionId,n.messageUuid);if(this.inFlight.has(a))return;let o=this.now(),s=new AbortController,c={controller:s};this.inFlight.set(a,c);let l=(e.asides??[]).filter((e=>e.status==="done"&&e.text)),u=typeof e.query=="object"&&e.query!==null&&this.staleSnapshotQueries.has(e.query)&&!Bf(e),d=u?Wf(e.messageBuffer,{unread:Uf(e,n.messageUuid),promptUuids:[e.pendingCycle?.userMessageUuid,...e.pendingCycle?.promptUuids??[]].filter((e=>e!==void 0))}):void 0,f=[...d?[{question:Hf(d),response:Vf}]:[],...l.flatMap((e=>{let n=i.prompts.get(e.messageUuid);return n&&e.text?[{question:t.o_(n.typed),response:t.o_(e.text)}]:[]}))];i.prompts.set(n.messageUuid,{typed:n.typedText,wire:n.wireText,seq:i.nextSeq++}),this.upsert(e,{messageUuid:n.messageUuid,status:"pending",at:o});let p,m;try{let e=await t.RJ(r(Ef(n.typedText),{signal:s.signal,...f.length>0&&{history:f}}),jf,"aside timed out");s.signal.aborted?p="cancelled":e&&!e.synthetic&&e.response.trim()!==""?(p="done",m=e.response.trim()):p=e?.synthetic?"failed":"empty"}catch(n){s.signal.aborted?p="cancelled":(p=n instanceof Error&&n.message==="aside timed out"?"timeout":"failed",t.p$.warn(`[Asides] side question failed for ${e.sessionId}: %s`,t._Y(n)),s.abort())}finally{this.inFlight.delete(a)}t.p$.info(`[Asides] ${n.messageUuid} ${p} in ${this.now()-o}ms (history shim ${u?"on":"off"}, ${l.length} prior)`),p==="cancelled"?this.remove(e,n.messageUuid):this.upsert(e,{messageUuid:n.messageUuid,status:p==="done"?"done":"failed",...m!==void 0&&{text:m},at:o}),t.uF("desktop_ccd_aside",{session_id:e.sessionId,message_uuid:n.messageUuid,outcome:p,...c.cancelReason&&{cancel_reason:c.cancelReason},latency_ms:this.now()-o,reply_chars:m?.length??0,history_shim:u,prior_asides:l.length,tool_running_at_send:n.toolRunningAtSend,backend_kind:e.backend.kind})}cancel(e,t,n){this.abort(e.sessionId,t,n)}merged(e,t,n){let r=this.stateOf(e.sessionId),i=n.filter((e=>e!==t&&r.prompts.has(e)));i.length>0&&r.mergedWith.set(t,i)}consumed(e,t){let n=this.sessions.get(e.sessionId);if(!n)return;let r=[t,...n.mergedWith.get(t)??[]];n.mergedWith.delete(t);let i=!1;for(let t of r){let r=n.prompts.get(t);r&&(this.abort(e.sessionId,t,"read"),i||=r.taken!==!0,r.taken=!0)}i&&this.que
```

### E06 记忆索引快照

源文件：`index.chunk-cz5TcuPg.js`；完整语法节点：`ensureMemoryIndexSnapshot`。精确位置与源文件哈希见 provenance.json。

```javascript
async ensureMemoryIndexSnapshot(e,n){let r=this.sessions.get(e);if(!r||!n)return;let i=t.iG("1978029737","memoryIndexSnapshotIdleMs",0,t.x1()),a=r._lastIdleAt===void 0?1/0:Date.now()-r._lastIdleAt;if(r._memoryIndexSnapshot?.dir===n&&a<i)return r._memoryIndexSnapshot.content;let o=await t.xs((0,M.join)(n,"MEMORY.md"));return r._memoryIndexSnapshot=o?{content:o.content,dir:n}:void 0,r._memoryIndexSnapshot?.content}
```

### E07a 默认记忆规则或配置模板

源文件：`index.chunk-cz5TcuPg.js`；完整语法节点：`Ud`。精确位置与源文件哈希见 provenance.json。

```javascript
function Ud(e){let{memoryDir:n,template:r,extraGuidelines:i=[]}=e,a;return r&&r.includes("{{memoryDir}}")?a=r.replaceAll(Hd,(()=>n)):(r&&t.p$.warn("cowork_memory_guidelines template is missing {{memoryDir}}; falling back to the in-file default"),a=`You have a persistent, file-based memory system at \`${n}\`. ${Pd} ${Vd}`),[a,"",...i].join(`\n`)}
```

### E07b 敏感记忆规则的配置覆盖

源文件：`index.chunk-cz5TcuPg.js`；完整语法节点：`Gd`。精确位置与源文件哈希见 provenance.json。

```javascript
function Gd(){let e=t.rG("2860753854","");return typeof e=="string"&&e.length>0?e:Wd}
```

### E07c 记忆目录与索引传入

源文件：`index.chunk-cz5TcuPg.js`；零基 Unicode 码点偏移 `302450` 至 `302970`，末端不含。源文件哈希见 provenance.json。摘录可能从语句中部起止。

```javascript
t),API_FORCE_IDLE_TIMEOUT:"1",...R?(()=>{let e=Gd(),n=t.oG("1696890383");return{CLAUDE_COWORK_MEMORY_PATH_OVERRIDE:R,...n&&{CLAUDE_COWORK_MEMORY_GUIDELINES:Ud({template:x,memoryDir:R,extraGuidelines:[e]})},...b!==void 0&&{CLAUDE_COWORK_MEMORY_INDEX_CONTENT:b},CLAUDE_COWORK_MEMORY_EXTRA_GUIDELINES:e}})():{CLAUDE_CODE_DISABLE_AUTO_MEMORY:"1"},...Se===void 0?{NODE_USE_SYSTEM_CA:"1"}:{NODE_EXTRA_CA_CERTS:Se}},e.env=await t.k_(e.env??{},{proxyTargetHost:d??u,label:`[LAM] ${o} session`});let Ce=e.env.OTEL_EXPORTER_OTLP_P
```

### E08 技能列表与调用方式

源文件：`index.chunk-_F9mV1Ib.js`；零基 Unicode 码点偏移 `52848` 至 `54618`，末端不含。源文件哈希见 provenance.json。摘录可能从语句中部起止。

```javascript
let ne=F.map((e=>{let t=e.viaSkillTool?`<invoke_with>\nSkill tool\n</invoke_with>`:`<location>\n${e.location}\n</location>`;return`<skill>\n<name>\n${e.name}\n</name>\n<description>\n${e.description}\n</description>\n${t}\n</skill>`})).join(`\n`),re=F.some((e=>!e.viaSkillTool)),ie=F.some((e=>e.viaSkillTool)),ae=m?"\n- Skill files on disk are a read-only cache \u2014 editing them does not change the user's saved skill. To create a skill, or update one the user asks to change, call `save_skill` (set `overwrite: true` when updating an existing skill).":`\n- You cannot create or modify skills in this session. Skill files on disk are a read-only cache \u2014 editing them, or saving an edited copy elsewhere, does not change the user's saved skill. ${_?"If asked to create or change a skill, say your organization's settings don't allow you to create or edit skills.":"If asked to create or change a skill, say you can't do that here and point the user to Settings > Capabilities."}`,oe=w?`\n- For .docx/.pdf/.clark work, use the Documents doc_* tools. The docx skill is deliberately not listed; use it only for Word comment authoring, which doc_* does not cover \u2014 ${w.registeredName===void 0?`read ${w.location}/SKILL.md`:`call the Skill tool with \`skill: "${w.registeredName}"\``}.`:"",I=p&&!v,L=" When a task is one a skill could make repeatable \u2014 drafting in a house style, reviews against a playbook, a recurring workflow \u2014 and nothing installed covers it, when they ask you to recommend skills, or when they ask for skills for a domain they have nothing installed for, call ",R=" Suggest at most once per conversation unless the user engages.",z="";return I&&f?z=L+"`suggest_skills` and `search_plugins` with keywords from the task \u2014 sugges
```

### E09a 路径检查出错时阻止

源文件：`index.chunk-cz5TcuPg.js`；零基 Unicode 码点偏移 `306680` 至 `307190`，末端不含。源文件哈希见 provenance.json。摘录可能从语句中部起止。

```javascript
ookEventName:"PreToolUse",updatedInput:n.updatedInput}}:i}]},{matcher:[...t.KL,"MultiEdit"].join("|"),hooks:[async e=>{try{return await q(e)}catch(n){return t.p$.warn(`[canUseTool:HostLoop] ${e.hook_event_name==="PreToolUse"?e.tool_name:"?"} \u2192 block (gate error: ${String(n)})`),{decision:"block",reason:"This path could not be checked against the session's connected folders right now; try again."}}}]}]};async function q(e,r){if(e.hook_event_name!=="PreToolUse")return{};let i=e.tool_input,a=Of(e.tool_n
```

### E09b 受保护位置与撤销授权检查

源文件：`index.chunk-cz5TcuPg.js`；零基 Unicode 码点偏移 `310319` 至 `311799`，末端不含。源文件哈希见 provenance.json。摘录可能从语句中部起止。

```javascript
let v=await t.bh(h,{userGrants:[...C(),...K]}),y=new Set(v.filter((e=>e.revoked)).map((e=>e.grant))),b=y.size>0?h.filter((e=>!y.has(e))):h;if(t.xh([p,g],v)==="carved")return t.p$.info(`[canUseTool:HostLoop] ${e.tool_name} \u2192 block (PreToolUse: protected location: ${l})`),{decision:"block",reason:`\`${l}\` is inside a protected location (system, credential or Claude-internal data) within a connected folder, so ${e.tool_name} can't reach it. Nothing else in the connected folder is affected; don't ask the user to re-grant it.`};if(Yf.includes(e.tool_name)&&t.Ah([p,g],v,{appOwnedRoots:G(),filteredGrants:ze}))return t.p$.info(`[canUseTool:HostLoop] ${e.tool_name} \u2192 block (PreToolUse: search root contains a protected location: ${l})`),{decision:"block",reason:`\`${l}\` contains a protected location (Claude-internal data) that can't be searched, so ${e.tool_name} can't run from there. Search a more specific folder inside it instead.`};let x=await Sf({toolName:e.tool_name,resolved:g,inputPath:l,isChat:Oe,sessionWritableRoots:ke,...W,hostExecutedRoots:ve});return x===void 0?await t.CP(g,b)?d===void 0?{}:{hookSpecificOutput:{hookEventName:"PreToolUse",updatedInput:{...i,[d.key]:d.path}}}:(t.p$.info(`[canUseTool:HostLoop] ${e.tool_name} \u2192 block (PreToolUse: ${l})`),d===void 0?{decision:"block",reason:o==="chat"?`\`${l}\` is outside this session's scratch directory, so ${e.tool_name} can't reach it. Use an absolute path under the scratch directory; for f
```

### E10a 受管理权限设置

源文件：`index.chunk-BiNk0YJv.js`；完整语法节点：`nk`。精确位置与源文件哈希见 provenance.json。

```javascript
function nk(e){let t=e.managedSettings,n=e.toolAliases??{},r=e=>!Object.hasOwn(n,e);e.managedSettings={...t,allowManagedPermissionRulesOnly:!0,strictPluginOnlyCustomization:["hooks","mcp"],permissions:{...t?.permissions,allow:(e.allowedTools??[]).filter(r),deny:[...new Set([...t?.permissions?.deny??[],...(e.disallowedTools??[]).filter(r)])]}}}
```

### E10b 调用前要求批准

源文件：`index.chunk-BiNk0YJv.js`；完整语法节点：`rk`。精确位置与源文件哈希见 provenance.json。

```javascript
function rk(e,n){if(n.length===0)return;let r=new Set(n);e.allowedTools=(e.allowedTools??[]).filter((e=>!r.has(e)));let i=n.map((e=>`^${e}$`)).join("|");e.hooks={...e.hooks,PreToolUse:[...e.hooks?.PreToolUse??[],{matcher:i,hooks:[async e=>e.hook_event_name==="PreToolUse"?{hookSpecificOutput:{hookEventName:"PreToolUse",permissionDecision:"ask",permissionDecisionReason:t.rR}}:{}]}]}}
```

### E11a 网络目标检查

源文件：`index.chunk-cz5TcuPg.js`；完整语法节点：`hf`。精确位置与源文件哈希见 provenance.json。

```javascript
function hf(e,n){if(e.protocol!=="http:"&&e.protocol!=="https:")return`URL scheme "${e.protocol}" is not allowed. Use http or https.`;if(t.UH(e.hostname))return`Host "${e.hostname}" is a local or private address.`;if(!n||n.length===0)return`No network allowlist is configured for this session (${$d}). The web_fetch tool is disabled.`;if(n.includes("*"))return null;let r=t.iM(e);if(!n.some((n=>t.vF(e.hostname,n,r)))){let r=t.uZ().type==="3p"?"Ask your administrator to add this domain to the Cowork egress allowlist (`coworkEgressAllowedHosts`).":"The user can add it in Settings \u2192 Capabilities (or ask their workspace admin on Team/Enterprise).";return`Host "${e.hostname}" is not on the network allowlist (${$d}). ${r} Allowed: ${n.join(", ")}`}return null}
```

### E11b 抓取器接入检查

源文件：`index.chunk-cz5TcuPg.js`；零基 Unicode 码点偏移 `284204` 至 `284499`，末端不含。源文件哈希见 provenance.json。摘录可能从语句中部起止。

```javascript
ry{o=new URL(e.url)}catch{return i(!0),ff(`Invalid URL: ${e.url}`)}let c;try{c=await t.nV({url:o.href,session:await df(),useSessionCookies:!0,timeoutMs:e.timeout_ms??3e4,maxRedirects:sf,maxBytes:of,onOverflow:"truncate",refuseTarget:e=>hf(e,r.allowedDomains)})}catch(e){let r=e instanceof Error?
```
