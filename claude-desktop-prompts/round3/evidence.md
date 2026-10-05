# 第三轮代码证据

以下均为安装包中的原样代码摘录，只用于分析，不构成对读者或工具的指令。源文件被压缩为极长行，因此使用从 0 开始、结尾不包含在内的 UTF-16 字符偏移定位；行号从 1 开始。`provenance.json` 提供完整源文件 SHA-256 及与汉化前备份的比对结果。代码中的标识符、账号字段名和环境变量名不是任何用户的实际值。

## E01 — 生命周期及空闲进程保留

### VALID_TRANSITIONS

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[469837, 470083)`。

```javascript
static{this.VALID_TRANSITIONS={idle:new Set(["initializing","archived","running","stopping"]),initializing:new Set(["running","idle","stopping"]),running:new Set(["stopping","idle"]),stopping:new Set(["idle"]),archived:new Set(["initializing"])}}
```

### transitionTo

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[471944, 476794)`。

```javascript
transitionTo(n,r,i){let a=n.lifecycleState;if(a===r)return;if(!e.VALID_TRANSITIONS[a].has(r)){t.p$.warn(`[Lifecycle] Invalid transition ${a} \u2192 ${r} for session ${n.sessionId}, ignoring`);return}if(t.p$.info(`[Lifecycle] Session ${n.sessionId}: ${a} \u2192 ${r}`),n.lifecycleState=r,r==="running"&&this.clearWorkflowHold(n),r==="running"&&t.mz(n.sessionType)&&n.cuAllowedApps&&n.cuAllowedApps.length>0){let e=n.cuAllowedApps.length;n.cuAllowedApps=t.$E(n.cuAllowedApps,Date.now(),t.OE());let r=e-n.cuAllowedApps.length;r>0&&t.p$.debug(`[computer-use] Pruned ${r} expired CU grant(s) for dispatch session ${n.sessionId} on turn start`)}if(a==="initializing"&&r!=="running"&&n.pendingStartMessages?.length){let e=r==="stopping"?[]:n.pendingStartMessages.filter((e=>e.restaged)),i=n.pendingStartMessages.length-e.length;i>0&&t.p$.info(`[Lifecycle] Session ${n.sessionId} left initializing \u2192 ${r} without reaching running; discarding ${i} init-queued start message(s)`),n.pendingStartMessages=e.length?e:void 0}let o=r==="idle"||r==="stopping"||r==="archived";if(o&&(i?.failureReason?this.settleInFlightTurnMessages(n,"restage",i.failureReason):r==="stopping"||r==="archived"?this.settleInFlightTurnMessages(n,"discard",`clean ${r}`):n._turnInterruptRequested===!0&&n._turnTokenCapError===void 0&&this.settleInFlightTurnMessages(n,"discard","clean idle (user stop)")),r==="running"&&process.platform==="darwin"){let e=n.sessionId;(async()=>{let{onSessionStartedDriving:t}=await Promise.resolve().then((()=>require("./index.chunk-BcYASoX4.js"))).then((e=>e.dO));t(e)})().catch((e=>{t.p$.warn("grandPrix agentic-mode turn-start mark failed",{error:e instanceof Error?e.name:"unknown"})}))}if(t.Px.currentHolder===n.sessionId&&o&&(t.Px.release(n.sessionId),this.emit("event",{type:"cu_lock_released",sessionId:n.sessionId}),t.lF("cu_lock_released",{session_id:n.sessionId,session_type:"cowork",held_duration_ms:n.cuLockAcquiredAt?Date.now()-n.cuLockAcquiredAt:0,release_trigger:r,was_teach_mode:n.teachModeActive??!1}),n.cuLockAcquiredAt=void 0),o){t.Px.release(n.sessionId),n.cicOnceApproved=void 0,n.activeSkillThisTurn=void 0,n.teachModeActive&&(t.lF("cu_teach_session",{session_id:n.sessionId,session_type:"cowork",duration_ms:n.teachModeEnteredAt?Date.now()-n.teachModeEnteredAt:0,exit_trigger:r}),n.teachModeEnteredAt=void 0,n.teachModeActive=!1,this.resolveTeachStep({action:"exit"}),this.emit("teachModeChanged",{sessionId:n.sessionId,active:!1}));let e=n.cuHiddenDuringTurn;e&&e.size>0&&(t.LG("chicagoAutoUnhide")&&t.Yb([...e]).catch((e=>t.p$.warn("[computer-use] auto-unhide on leavingRunning failed",e))),n.cuHiddenDuringTurn=void 0),n.cuHiddenPendingNote=void 0,n.cuMentionedWindows=void 0,n.widgetToolStates=void 0,t.Af(n)||t.Pp(n.sessionId);let i=n.cuClipboardStash;if(n.cuClipboardStash=void 0,i!==void 0&&t.Jb(i).catch((e=>{t.p$.warn("[computer-use] clipboard restore on leavingRunning failed",e)})),this.permissionRouter.denyPendingPermissionsForSession(n.sessionId,"Turn ended"),process.platform==="darwin"){let e=n.sessionId,i=r==="archived";(async()=>{let{onSessionStoppedDriving:t}=await Promise.resolve().then((()=>require("./index.chunk-BcYASoX4.js"))).then((e=>e.dO));await t(e,{dropSession:i})})().catch((e=>{t.p$.warn("grandPrix agentic-mode turn-end relay failed",{error:e instanceof Error?e.name:"unknown"})}))}}if(r==="archived"&&n.messageBuffer.length>0&&this.releaseBufferIntoTurnCount(n),r==="archived"&&(n.pendingStartMessages=void 0,this.transcriptCache.delete(n.sessionId)),this.emit("lifecycleChanged",{sessionId:n.sessionId,oldState:a,newState:r,errorMessage:i?.error,errorCategory:i?.errorCategory}),r==="idle"){n._turnInterruptRequested=void 0,n._turnTokenCapError=void 0,n.sessionType===void 0&&!this.hasRunningMemorySession()&&this.memorySync?.refreshIfStale(n.spaceId||void 0);let e=this.warmLifecycle.getTimeoutMs(),r=!i?.error&&a==="running",o=!!n.query&&!!n.inputStream;n._lastIdleAt=Date.now();let s=t.Af(n);(e>0||s)&&r&&o&&n.sessionType!=="dispatch_child"&&!this.heldProcessPolicyVeto(n)?(n._workflowResultsPending&&this.armWorkflowResultsTurnHold(n),e>0?this.warmLifecycle.onTurnComplete(n.sessionId):t.p$.info(`[Lifecycle] Idle grace disabled for ${n.sessionId} but a Workflow run is live \u2014 keeping process for its task_notification`),n.mcpServersDirty&&n.activeMcpServers&&(n.mcpServersDirty=!1,n.query?.setMcpServers(this.mcpCoordinator.sealForSdk(n.sessionId,n.hostLoopMode!==!0||n._startedThroughHostCliLauncher===!0,n.activeMcpServers)).catch((e=>t.p$.warn(`[LAM] Deferred setMcpServers failed for ${n.sessionId}: %o`,e))))):this.teardownIdleProcess(n),this.syncGlobalMemoryBack(n.sessionId),i?.failureReason?this.bookTurnFailure(n,{error:i.error,failureReason:i.failureReason,errorCategory:i.errorCategory,isAfterStop:i.isAfterStop}):this.recordSessionError(n,i?.error,i?.errorCategory)}}
```

## E02 — 清理 query 与关闭进程的路径

### teardownIdleProcess

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[465349, 465969)`。

```javascript
teardownIdleProcess(e){this.settleInFlightTurnMessages(e,"restage","process-teardown"),this.persistGrowthBookCacheFromSession(e.sessionId),this.persistArtifactRosterFromSession(e.sessionId),sa(e,"Lifecycle"),e.inputStream=null,e.activeMcpServers=void 0,this.clearBackgroundTaskState(e)&&this.emit("event",{type:"session_updated",sessionId:e.sessionId}),e.vmProcessId&&(e._priorVmProcessId=e.vmProcessId),e.vmProcessId=void 0,this.stopFileWatching(e.sessionId),T.t.getPluginMcpInstance().closeRuntimePluginServersForSession(e.sessionId).catch((e=>{t.p$.warn("[Lifecycle] runtime plugin server close failed",{error:e})}))}
```

### sa

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[82391, 82637)`。

```javascript
function sa(e,n){if(e.query)try{e.query.close()}catch(r){t.p$.warn(`[${n}] Failed to close query for session ${e.sessionId}:`,r),e.killHostLoopProcessGroup?.()}e.query&&(e._queryClosedAt=Date.now()),e.query=null,e.killHostLoopProcessGroup=void 0}
```

## E03 — 停止和中断

### stopSession

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[510970, 512161)`。

```javascript
async stopSession(e,n=!1){let r=this.sessions.get(e);if(!r)return;r.initGen=(r.initGen??0)+1,r._deliberateStopGen=(r._deliberateStopGen??0)+1,t.p$.info(`Stopping session ${e}`),r._suggestionTimeout&&=(clearTimeout(r._suggestionTimeout),void 0),r.promptSuggestion=void 0,r.initializationStatus=void 0,this.clearBackgroundTaskState(r);let i=t.Mf(r)||r.query;this.teardownWarmIfIdle(r,"user-stop"),this.transitionTo(r,"stopping"),r.inputStream&&r.inputStream.done(),r.pendingStartMessages=void 0,this.transitionTo(r,"idle");let a={type:"close",sessionId:e,code:0};this.emit("event",a),this.releaseBufferIntoTurnCount(r),this.transcriptCache.delete(e);let o=r.cachedTotalTurns??0;if(br(e),vi(e),this.mcpCoordinator.unregisterRootsProvider(e),await T.t.getPluginMcpInstance().closeRuntimePluginServersForSession(e).catch((e=>{t.p$.warn("[stopSession] runtime plugin server close failed",{error:e})})),i&&!n){let n=Date.now()-r.createdAt,i=await this.getTranscriptSizeBytes(r);t.lF("lam_session_stopped",{session_id:e,cli_session_id:r.cliSessionId??null,vm_instance_id:t.dv(),session_type:r.sessionType,total_turns:o,session_duration_ms:n,transcript_size_bytes:i})}this.closeSessionAuditLogger(e)}
```

### interruptTurn

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[505657, 506644)`。

```javascript
async interruptTurn(e){let n=[];for(let t of this.sessions.values())t.parentSessionId===e&&n.push(t.sessionId);n.length>0&&await Promise.allSettled(n.map((e=>this.interruptTurn(e))));let r=await Promise.resolve().then((()=>require("./index.chunk-BxH9TV_3.js"))),i=r.claudeCodeSessionManager.getSessionsByDispatchParent(e);i.length>0&&await Promise.allSettled(i.filter((e=>e.lifecycleState==="running")).map((e=>r.claudeCodeSessionManager.stopSession(e.sessionId))));let a=this.sessions.get(e);if(!a?.query){t.p$.debug(`[interruptTurn] Session ${e} has no active query, no-op`);return}if(a.lifecycleState!=="running"){t.p$.debug(`[interruptTurn] Session ${e} is ${a.lifecycleState}, not running \u2014 no-op`);return}t.p$.info(`[interruptTurn] Interrupting session ${e}`),a._turnInterruptRequested=!0,a._turnTokenCapError===void 0&&(a._deliberateStopGen=(a._deliberateStopGen??0)+1);try{await a.query.interrupt()}catch(n){t.p$.warn(`[interruptTurn] Failed to interrupt session ${e}:`,n)}}
```

## E04 — 启动消息重投与主动停止代数

### drainPendingStartMessages

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[482640, 483814)`。

```javascript
drainPendingStartMessages(e){let n=e.pendingStartMessages;if(!n?.length)return;e.pendingStartMessages=void 0,t.p$.info(`Draining ${n.length} queued start message(s) for session ${e.sessionId}`);let r=this.healthMonitor.getReapClock(),i=e._deliberateStopGen;(async()=>{for(let[a,o]of n.entries()){if(e._deliberateStopGen!==i){t.p$.info(`Dropping ${n.length-a} queued start message(s) for session ${e.sessionId}: a stop, interrupt or rewind happened before they were delivered`);return}if(o.restaged&&o._isUnattended&&il(r,o.sentAt)>18e5){t.p$.info(`Dropping stale restaged unattended message for session ${e.sessionId} (uuid=${o.messageUuid??"none"})`);continue}try{await this.sendMessage(e.sessionId,o.message,o.images,o.userSelectedFiles,o.messageUuid,{channel:o.channel,ttftSentAtOverride:o.sentAt,contentBlocks:o.contentBlocks,_isUnattended:o._isUnattended,_restagedRedelivery:o.restaged,_queuedRedelivery:!0,seededSummon:o._seededSummon,seededKind:o._seededKind})}catch(n){t.p$.error(`Failed to deliver queued start message for session ${e.sessionId} (uuid=${o.messageUuid??"none"}):`,n),o.restaged&&e._deliberateStopGen===i&&(e.pendingStartMessages??=[]).push(o)}}})()}
```

## E05 — 保存与恢复会话记录的代码，不是实际用户记录

### writeSessionToDisk

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[450673, 454014)`。

```javascript
async writeSessionToDisk(e){if(!this.sessions.has(e.sessionId))return;let r=this.getSessionFilePath(e.sessionId);if(!r){t.p$.warn("Cannot save session: storage path not available");return}await this.ensureStorageDir();try{let i=Array.from(e.fsDetectedFiles.values()),a={sessionId:e.sessionId,processName:e.processName,cliSessionId:e.cliSessionId,cwd:e.cwd,userSelectedFolders:t.If(e),resolvedFolderKinds:(e.resolvedFolders??[]).map(t.Yq),createdAt:e.createdAt,lastActivityAt:e.lastActivityAt,model:e.model,permissionMode:e.permissionMode,isArchived:e.lifecycleState==="archived",title:e.title,userApprovedFileAccessPaths:e.userApprovedFileAccessPaths,vmProcessName:e.vmProcessName,hostLoopMode:e.hostLoopMode,importedFrom:e.importedFrom,resumeConfirmed:e.resumeConfirmed,relinkedToSpace:e.relinkedToSpace,spacePlacedByUser:e.spacePlacedByUser,globalMemoryBaselineHash:e.globalMemoryBaselineHash,webFetchAllowedUrls:e.webFetchAllowedUrls&&e.webFetchAllowedUrls.size>0?Array.from(e.webFetchAllowedUrls):void 0,error:e.error,errorCategory:e.errorCategory,errorAt:e.errorAt,errorVersion:e.errorVersion,initialMessage:e.initialMessage,slashCommands:e.slashCommands,mcqAnswers:e.mcqAnswers,enabledMcpTools:e.enabledMcpTools,remoteMcpServersConfig:n.Lt(e.remoteMcpServersConfig),fsDetectedFiles:i.length>0?i:void 0,fileDeleteApprovedMounts:e.fileDeleteApprovedMounts,folderMountNames:e.folderMountNames,chromePermissionMode:e.chromePermissionMode,chromeAllowedDomains:e.chromeAllowedDomains,chromeTabGroupId:e.chromeTabGroupId,chromePermsBeforeUnsupervised:e.chromePermsBeforeUnsupervised,cuAllowedApps:e.cuAllowedApps,cuGrantFlags:e.cuGrantFlags,cuFlagsGrantedAt:e.cuFlagsGrantedAt,approvedToolNames:e.approvedToolNames,effortOverride:e.effortOverride,pinnedSpawnEffort:e.pinnedSpawnEffort,cuLastScreenshotDims:e.cuLastScreenshotDims,cuSelectedDisplayId:e.cuSelectedDisplayId,egressAllowedDomains:e.egressAllowedDomains,withheldConnectorHosts:e.withheldConnectorHosts,orgCliExecPolicies:e.orgCliExecPolicies,otelConfig:e.otelConfig,orgOtlpContentCapture:e.orgOtlpContentCapture,memoryEnabled:e.memoryEnabled,skillsEnabled:e.skillsEnabled,pluginsEnabled:e.pluginsEnabled,documentFunnelEnabled:e.documentFunnelEnabled,docxEditingCarveout:e.docxEditingCarveout,frameArtifactsEnabled:e.frameArtifactsEnabled,artifactHostGrant:e.artifactHostGrant,pluginInstallPaths:e.pluginInstallPaths,scheduledTaskId:e.scheduledTaskId,scheduledRunContinued:e.scheduledRunContinued,spaceId:e.spaceId,spaceIdSetBy:e.spaceIdSetBy,userSelectedProjectUuids:e.userSelectedProjectUuids,isStarred:e.isStarred,sessionType:e.sessionType,parentSessionId:e.parentSessionId,dispatchParentOrigin:e.dispatchParentOrigin,outboundCCRRemoteId:e.outboundCCRRemoteId,promptSuggestion:e.promptSuggestion,pendingNotifications:e.pendingNotifications.length>0?e.pendingNotifications:void 0,systemPrompt:e.systemPrompt,systemPromptRendererAppends:e.systemPromptRendererAppends,accountName:e.accountName,emailAddress:e.emailAddress,imagineSystemPrompt:e.imagineSystemPrompt,memoryGuidelinesTemplate:e.memoryGuidelinesTemplate,coworkSyspromptMap:pn(e.model,e.coworkSyspromptMap),spSectionPrompts:e.spSectionPrompts};if(!this.sessions.has(e.sessionId))return;await t.P$(r,a),t.p$.debug(`Saved session ${e.sessionId} to storage`)}catch(n){t.p$.error(`Failed to save session ${e.sessionId}:`,n)}}
```

### loadSessions

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[445292, 449983)`。

```javascript
async loadSessions(){this.unreadableSessionIds.clear();let r=this.getAccountStorageDir();if(!r){t.p$.info("No persisted sessions found");return}let i=0,a=n.Mt(),o=e=>{this.unreadableSessionIds.add(e.slice(0,-5))},s=async(e,r)=>{try{let s=vm(await(0,I.readFile)(e,"utf-8"));if(!s){t.p$.warn(`Skipping invalid session file: ${r}`),o(r);return}let c=C.y(s.folderMountNames??C.g(s.userSelectedFolders??[],s.hostLoopMode===!0)),l=(await Promise.all((s.userSelectedFolders||[]).map((async e=>await(0,I.access)(e).then((()=>!1),(e=>e.code==="ENOENT"))?(t.p$.info(`Filtering out deleted folder from session ${s.sessionId}: ${e}`),null):e)))).filter((e=>e!==null)),u=t.uP(l,(e=>{t.p$.warn(`[Restore] Dropping persisted folder outside allowed mount roots from session ${s.sessionId}: ${e.folderPath}`)})),d=new Map;if(s.fsDetectedFiles)for(let e of s.fsDetectedFiles)d.set(e.hostPath,e);let f=t.WY(s.model),p={sessionId:s.sessionId,processName:s.processName,cliSessionId:s.cliSessionId,cwd:s.cwd,resolvedFolders:u.map((e=>{let n=s.resolvedFolderKinds?.find((t=>t.display===e));return n?t.Iq(n):{kind:"local",display:e,canonical:e}})),query:null,inputStream:null,lifecycleState:s.isArchived?"archived":"idle",isFirstTurn:!1,messageBuffer:[],createdAt:s.createdAt,lastActivityAt:s.lastActivityAt,model:f,permissionMode:s.permissionMode,title:s.title??void 0,userApprovedFileAccessPaths:s.userApprovedFileAccessPaths,vmProcessName:s.vmProcessName??s.processName,hostLoopMode:s.hostLoopMode,importedFrom:s.importedFrom,resumeConfirmed:s.resumeConfirmed,relinkedToSpace:s.relinkedToSpace,spacePlacedByUser:s.spacePlacedByUser,globalMemoryBaselineHash:s.globalMemoryBaselineHash,webFetchAllowedUrls:s.webFetchAllowedUrls?new Set(s.webFetchAllowedUrls):void 0,error:s.error,errorCategory:s.errorCategory,errorAt:s.errorAt,errorVersion:s.errorVersion,initialMessage:s.initialMessage,slashCommands:s.slashCommands,mcqAnswers:s.mcqAnswers,enabledMcpTools:s.enabledMcpTools,remoteMcpServersConfig:a(n.Nt(s.remoteMcpServersConfig)),fsDetectedFiles:d,fileDeleteApprovedMounts:s.fileDeleteApprovedMounts,folderMountNames:c,chromePermissionMode:s.chromePermissionMode,chromeAllowedDomains:s.chromeAllowedDomains,chromeTabGroupId:s.chromeTabGroupId,chromePermsBeforeUnsupervised:s.chromePermsBeforeUnsupervised,cuAllowedApps:s.cuAllowedApps,cuGrantFlags:s.cuGrantFlags,cuFlagsGrantedAt:s.cuFlagsGrantedAt,approvedToolNames:s.approvedToolNames,effortOverride:s.effortOverride,pinnedSpawnEffort:s.pinnedSpawnEffort,cuLastScreenshotDims:s.cuLastScreenshotDims,cuSelectedDisplayId:s.cuSelectedDisplayId,egressAllowedDomains:s.egressAllowedDomains,withheldConnectorHosts:s.withheldConnectorHosts,orgCliExecPolicies:s.orgCliExecPolicies,otelConfig:s.otelConfig,orgOtlpContentCapture:s.orgOtlpContentCapture,memoryEnabled:s.memoryEnabled,skillsEnabled:s.skillsEnabled,pluginsEnabled:s.pluginsEnabled,documentFunnelEnabled:s.documentFunnelEnabled,docxEditingCarveout:s.docxEditingCarveout,frameArtifactsEnabled:s.frameArtifactsEnabled,artifactHostGrant:s.artifactHostGrant,pluginInstallPaths:s.pluginInstallPaths,scheduledTaskId:s.scheduledTaskId,scheduledRunContinued:s.scheduledRunContinued===!0||void 0,spaceId:s.spaceId,spaceIdSetBy:s.spaceIdSetBy,userSelectedProjectUuids:s.userSelectedProjectUuids,isStarred:s.isStarred,sessionType:s.sessionType,parentSessionId:s.parentSessionId,dispatchParentOrigin:s.dispatchParentOrigin,outboundCCRRemoteId:s.outboundCCRRemoteId,promptSuggestion:s.promptSuggestion,pendingNotifications:s.pendingNotifications??[],systemPrompt:s.systemPrompt,systemPromptRendererAppends:s.systemPromptRendererAppends,accountName:t.uZ().type==="3p"?void 0:s.accountName,emailAddress:s.emailAddress,imagineSystemPrompt:s.imagineSystemPrompt,memoryGuidelinesTemplate:s.memoryGuidelinesTemplate,coworkSyspromptMap:s.coworkSyspromptMap,spSectionPrompts:s.spSectionPrompts};if(this.sessions.has(s.sessionId)){t.p$.info(`[Restore] Skipping ${s.sessionId} \u2014 live session already in memory`);return}if(this.deletedSessionIds.has(s.sessionId)){t.p$.info(`[Restore] Skipping ${s.sessionId} \u2014 deleted during restore`);return}this.sessions.set(s.sessionId,p),i++}catch(e){t.p$.warn(`Failed to load session from ${r}:`,e),o(r)}},c=async e=>{try{return(await(0,I.readdir)(e)).filter((e=>e.startsWith("local_")&&e.endsWith(".json"))).map((t=>({filePath:(0,M.join)(e,t),file:t})))}catch(e){if(e.code==="ENOENT")return[];throw t.p$.error("Failed to load persisted sessions:",e),e}},l=(await Promise.all([c(r),c((0,M.join)(r,t.gL))])).flat();await new t.tJ({concurrency:e.SESSION_FILE_READ_CONCURRENCY}).addAll(l.map((({filePath:e,file:t})=>()=>s(e,t)))),t.p$.info(`Loaded ${i} persisted sessions from ${r}`)}
```

## E06 — SDK 会话续接、分支与回退

### resume / resumeSessionAt / forkSession

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[368350, 368861)`。

```javascript
let ct=z?.pendingRewindTo;z&&ct!==void 0?(ct?(q.resume=z.cliSessionId,q.resumeSessionAt=ct,q.forkSession=!0,t.p$.info(`[Rewind] resumeSessionAt=${ct} + forkSession=true for session ${a}`)):t.p$.info(`[Rewind] First-message rewind for session ${a} \u2014 starting fresh, same VM user`),z.transcriptFilePath=void 0):y&&b?(await b,q.resume=y.cliSessionIdToResume,t.p$.info(`[Branch] First spawn resumes copied transcript ${y.cliSessionIdToResume} for session ${a}`)):!l&&z?.cliSessionId&&(q.resume=z.cliSessionId);
```

## E07 — 待审批映射与审批结束清理

### pendingPermissions request schema

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[222003, 222384)`。

```javascript
this.pendingPermissions.set(b,{sessionId:e,ownerSessionId:s,toolName:r,telemetryToolName:d,telemetrySessionType:g,permissionSurface:h,input:i,suggestions:a,decisionReason:f,description:p,blockedPath:m,viaHostCliLauncher:u,scheduledRunApprovalDays:x,consentStamped:_,offersScheduledPublishGrant:v||void 0,requestedAt:Date.now(),resolve:e=>{o?.removeEventListener("abort",c),n(e)}});
```

### resolvePendingPermission

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[227685, 229867)`。

```javascript
resolvePendingPermission(e,r){let i=this.pendingPermissions.get(e);if(!i){t.p$.warn(`No pending permission request found for ${e} (bridge resolve)`);return}let a=this.delegate.getActiveSession(i.sessionId);if(r.behavior==="allow"&&r.updatedPermissions&&(t.pD(i.toolName)||this.answerStoresNothingLasting(i,a))){let{updatedPermissions:e,...t}=r;r=t}if(r.behavior==="allow"&&r.updatedPermissions){let e=t.eD(r.updatedPermissions,t.Hv(a));if(e!==r.updatedPermissions){let{updatedPermissions:t,...n}=r;r=e===void 0?n:{...n,updatedPermissions:e}}}if(i.viaHostCliLauncher===!0&&r.behavior==="allow"&&r.updatedPermissions){let e=t.cD(r.updatedPermissions);e!==r.updatedPermissions&&(r={...r,updatedPermissions:[...e]})}let o=r.behavior==="allow"?r.updatedPermissions?"always":"once":"deny";t.p$.info(`Bridge resolving permission ${e}: behavior=${r.behavior} (tool: ${i.toolName})`);let s=Date.now()-i.requestedAt;if(t.lF("lam_tool_permission_responded",{session_id:i.sessionId,session_type:i.telemetrySessionType??"cowork",user_message_uuid:a?.pendingUserMessageUuid??null,tool_name:i.telemetryToolName??i.toolName,request_id:e,permission_surface:i.permissionSurface,decision:o,latency_ms:s,permission_mode:a?.permissionMode??null}),this.pendingPermissions.delete(e),this.stampReapShieldOnAllow(i,r.behavior==="allow"),this.maybePersistWorkflowConsent(i,r),this.delegate.auditLog(i.sessionId,{type:"system",subtype:"permission_response",uuid:e,session_id:a?.cliSessionId,tool_name:i.toolName,decision:o,granted:r.behavior==="allow"}),a&&this.delegate.recordToolCall(a,i.toolName,r.behavior==="allow"),a&&r.behavior==="allow"&&r.updatedPermissions&&i.viaHostCliLauncher!==!0&&!(i.toolName==="Workflow"&&n.q(i.decisionReason))){a.scheduledTaskId&&!t.iS(i.toolName)&&!i.toolName.startsWith("plugin-shim:")&&t.Js.addApprovedPermissions(a.scheduledTaskId,r.updatedPermissions,{askedToolName:i.toolName});let e=t.QE(r.updatedPermissions);e.length>0&&(a.approvedToolNames=[...new Set([...a.approvedToolNames??[],...e])])}this.delegate.emit("event",{type:"tool_permission_resolved",sessionId:i.sessionId,request:{requestId:e,sessionId:i.sessionId,toolName:i.toolName,input:i.input}}),i.resolve(r)}
```

### denyPendingPermissionsForSession

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[229867, 230087)`。

```javascript
denyPendingPermissionsForSession(e,t,r){let i=[];for(let[t,n]of this.pendingPermissions)al(n,e)&&i.push(t);for(let e of i){let i={behavior:"deny",message:t};r&&n.Z(i,r),this.resolvePendingPermission(e,i)}return i.length}
```

## E08 — 历史审批复用与过期

### shouldAutoApprovePermission

来源：`index.chunk-BcYASoX4.js`，第 13 行，UTF-16 偏移 `[5750464, 5752908)`。

```javascript
shouldAutoApprovePermission(e,t,n){if(this.guardInitialized("shouldAutoApprovePermission"))return!1;if($z(t))return N.info(`${this.config.logPrefix} Not auto-approving "${t}" in scheduled task "${e}": no stored rule answers an Artifact-family ask`),!1;if(!n||n.length==0)return N.info(`${this.config.logPrefix} Not auto-approving "${t}" in scheduled task "${e}": no suggestions on request`),!1;let r=CRi(n);if(r.length===0)return N.info(`${this.config.logPrefix} Not auto-approving "${t}" in scheduled task "${e}": suggestions contained no allow-shaped addRules/replaceRules`),!1;if(t===TE)return N.info(`${this.config.logPrefix} Not auto-approving "${t}" in scheduled task "${e}": directory mounts use userSelectedFolders`),!1;if(Yur(t)||r.some((e=>Yur(e.toolName))))return N.info(`${this.config.logPrefix} Not auto-approving "${t}" in scheduled task "${e}": browser/computer sentinel permissions require a live card`),!1;if(r.some((e=>e.toolName.startsWith("plugin-shim:"))))return N.info(`${this.config.logPrefix} Not auto-approving "${t}" in scheduled task "${e}": plugin-shim rules are owned by account.settings.enabled_cli_ops`),!1;if(t===EE||G9t.includes(t))return N.info(`${this.config.logPrefix} Not auto-approving "${t}" in scheduled task "${e}": persistent account-wide writes require live approval`),!1;if(t.startsWith(hE))return N.info(`${this.config.logPrefix} Not auto-approving "${t}" in scheduled task "${e}": scheduled-task management tools require live approval`),!1;let i=this._scheduledTasks.get(e),a=(i?.approvedPermissions??[]).filter((e=>!dRi(e))),o=Date.now(),s=a.filter((e=>!this.isApprovalExpired(e,i?.createdAt,o))),c=this.givenBeforeRestrictions(),l=s.filter((e=>!c(e))),u=lRi(l),d=r.every(u);if(d)N.info(`${this.config.logPrefix} Auto-approved tool permission for "${t}" in scheduled task "${e}"`);else{let n=r.filter((e=>!u(e)));N.info(`${this.config.logPrefix} Not auto-approving "${t}" in scheduled task "${e}": rule(s) not covered by usable stored approvals: ${n.map((e=>e.toolName)).join(", ")} (usable=${l.length}, expired=${a.length-s.length}${l.length<s.length?`, given before the HIPAA-ready restrictions=${s.length-l.length}`:""}${n.some((e=>this.config.approvalLifetimeStoodIn?.(e.toolName)))?"; the lifetime reads 0 until the served configuration is fetched, or is unreadable":""})`),l.length<s.length&&n.every(lRi(s))&&eA("lam_hipaa_gate_blocked",{surface:"scheduled_task_stored_approvals"})}return d}
```

### isApprovalExpired

来源：`index.chunk-BcYASoX4.js`，第 13 行，UTF-16 偏移 `[5749563, 5749728)`。

```javascript
isApprovalExpired(e,t,n){let r=this.config.approvalLifetimeDays?.(e.toolName);if(r===void 0)return!1;let i=n-(e.approvedAt??t??NaN);return r<=0||!(i>=-3e5&&i<r*mRi)}
```

## E09 — 本地调度前置判断

### checkScheduledTasksInner

来源：`index.chunk-BcYASoX4.js`，第 13 行，UTF-16 偏移 `[5737838, 5743864)`。

```javascript
async checkScheduledTasksInner(){if(!this.runsAllowed()){N.debug(`${this.config.logPrefix} Scheduled tasks feature is disabled, skipping check`),this._notReadyTicks=0,this._offlineTicks=0;return}if(this._scheduledTasks.size===0){this._notReadyTicks=0,this._offlineTicks=0;return}let e=Date.now(),t=Array.from(this._pendingTaskDispatches.entries());for(let[n,r]of t){if(this._pendingTaskDispatches.get(n)!==r)continue;let{dispatchedAt:t,scheduledFor:i,ackedAt:a}=r;if(e-t>ARi){let r=this._scheduledTasks.get(n);if(r&&!r.cronExpression&&a!==void 0&&this._lastListenerReadyAt<=a)continue;let o=this._pendingJitterTimeouts.has(n);this.clearPendingDispatch(n),this.clearJitterTimeout(n),N.warn(`${this.config.logPrefix} Cleared stale pending dispatch for: ${n}${o?" (still in jitter delay; slot kept)":""}`),Q(`${this.config.telemetryPrefix}_scheduled_tasks_dispatch_stale`,{scheduled_task_id:n,pending_minutes:Math.round((e-t)/6e4),slot_consumed:!!r?.cronExpression&&!o,interval_minutes:zRi(r?.cronExpression)}),r?.cronExpression&&!o&&await this.updateLastRun(n,i)}}let n=XRi(e);if(n>0){N.info(`${this.config.logPrefix} Deferring check: ${Math.round(n/1e3)}s post-wake delay remaining`),this._notReadyTicks=0,this._offlineTicks=0;return}if(ZRi()){N.info(`${this.config.logPrefix} Deferring check: net.isOnline() is false`),this.hasAutoRunTasks()?this._offlineTicks++:this._offlineTicks=0,this._notReadyTicks=0;return}if(this._offlineTicks>=DRi&&Q(`${this.config.telemetryPrefix}_scheduled_tasks_offline`,{consecutive_ticks:this._offlineTicks}),this._offlineTicks=0,!this._rendererListenerReady){N.info(`${this.config.logPrefix} Renderer listener not ready, deferring check`);return}if(await this.heldByPlacement()){this._notReadyTicks=0;return}if(!await this.isReadyToDispatchBounded()){N.debug(`${this.config.logPrefix} Not ready to dispatch, skipping scheduled task check`);let e=this.autoRunTasks();e.length>0?(this._notReadyTicks++,this.config.onNotReadyToDispatch?.(this._notReadyTicks,ORi(e)),this._notReadyTicks===DRi&&Q(`${this.config.telemetryPrefix}_scheduled_tasks_not_ready`,{consecutive_ticks:this._notReadyTicks})):this._notReadyTicks=0;return}this._notReadyTicks=0;let r=this._dispatchThrottle&&this.hasAutoRunTasks()?this._dispatchThrottle("tick"):null;r?N.info(`${this.config.logPrefix} Deferring dispatch this tick: ${r}`):this._throttleDeferredSince.clear();let i,a=()=>i??=this.checkUnattendedCredentialBounded(),o=Array.from(this._scheduledTasks.values()),s=this._initCounter,c=new Map(o.map((e=>[e.id,this.dueSlotKey(e)])));N.debug(`${this.config.logPrefix} Checking ${o.length} scheduled tasks`);for(let e of o){if(!e.enabled||this.isDispatchBlocked(e.id))continue;let t=this._dispatchRetryBackoff.get(e.id);if(t){if(!this.dispatchAckModeActive())this._dispatchRetryBackoff.delete(e.id);else if(Date.now()<t.notBefore)continue}if(e.fireAt){if(!e.lastRunAt&&e.fireAt<=Date.now()){if(this.deferDueTaskThisTick(e.id,r)||this.shouldSkipDispatchForActiveSession(e.id))continue;try{await p.access(e.filePath)}catch{N.warn(`${this.config.logPrefix} Skipping scheduled task ${e.id}: task file not found at ${e.filePath}`);continue}let t=await this.gateDueTask(e,new Date(e.fireAt),a(),s,c.get(e.id),!0);if(t==="abort")return;if(t==="skip"||this.isDispatchBlocked(e.id))continue;this.markPendingDispatch(e.id,new Date(e.fireAt));let n=this.throttleDueDispatch(e.id);if(n){this.clearPendingDispatch(e.id),N.info(`${this.config.logPrefix} Dropped fireAt dispatch for ${e.id}: ${n}`);continue}await this.emitSessionStarted(await this.withFrontmatterModel(e),null,0)}continue}if(!e.cronExpression)continue;try{await p.access(e.filePath)}catch{N.warn(`${this.config.logPrefix} Skipping scheduled task ${e.id}: task file not found at ${e.filePath}`);continue}let n=WLi(e.cronExpression,e.lastRunAt),i=n?null:GLi(e.cronExpression,e.lastScheduledFor??e.lastRunAt,e.createdAt,e.missedRunScanFloor);if(s!==this._initCounter)return;let o=this.dueRunRetry(e,n||!!i);if(n||i||o){if(this.deferDueTaskThisTick(e.id,r)||this.shouldSkipDispatchForActiveSession(e.id))continue;let t=i??o?.slot??new Date;!i&&!o&&t.setUTCSeconds(0,0);let n=await this.gateDueTask(e,t,a(),s,c.get(e.id),!o);if(n==="abort")return;if(n==="skip"||this.isDispatchBlocked(e.id))continue;this.markPendingDispatch(e.id,t);let l=o?{proceed:!0,watcherReport:o.watcherReport}:await this.runWatcherGate(e);if(s!==this._initCounter){this.clearPendingDispatch(e.id);return}if(!this._pendingTaskDispatches.has(e.id))continue;if(!l.proceed){await this.updateLastRun(e.id,t,l.cursors),this.clearPendingDispatch(e.id);continue}let u=await this.withFrontmatterModel(e),d=e.model?void 0:u.model,f=i||o?0:this.getJitterSecondsForTask(e.id),p=f*1e3;if(p===0){let t=this.throttleDueDispatch(e.id);if(t){this.clearPendingDispatch(e.id),N.info(`${this.config.logPrefix} Dropped immediate dispatch for ${e.id}: ${t}`);continue}if(o&&s!==this._initCounter)return;if(o&&!this.beginRunRetry(e.id,o.attempt)){this.clearPendingDispatch(e.id),N.info(`${this.config.logPrefix} Dropped retry dispatch for ${e.id}: cancelled since it came due`);continue}await this.emitSessionStarted(u,i,0,l,o?.attempt)}else{this.clearJitterTimeout(e.id),N.info(`${this.config.logPrefix} Delaying dispatch for ${e.id} by ${f}s (jitter)`);let t=Date.now()+p,n=setTimeout((()=>{this._pendingJitterTimeouts.delete(e.id);let n=Date.now();if(KRi(t,n,this.config.logPrefix),XRi(n)>0){this.clearPendingDispatch(e.id),N.info(`${this.config.logPrefix} Dropped jitter fire for ${e.id}: post-wake defer`);return}if(ZRi()){this.clearPendingDispatch(e.id),N.info(`${this.config.logPrefix} Dropped jitter fire for ${e.id}: net.isOnline() is false`);return}let r=this.throttleDueDispatch(e.id);if(r){this.clearPendingDispatch(e.id),N.info(`${this.config.logPrefix} Dropped jitter fire for ${e.id}: ${r}`);return}let i=this._scheduledTasks.get(e.id);if(!i||!i.enabled){this.clearPendingDispatch(e.id);return}this.emitSessionStarted(i.model||!d?i:{...i,model:d},null,f,l)}),p);this._pendingJitterTimeouts.set(e.id,n)}}}}
```

## E10 — watcher 内容校验及触发阈值

### runWatcherGate

来源：`index.chunk-BcYASoX4.js`，第 13 行，UTF-16 偏移 `[5731920, 5733354)`。

```javascript
async runWatcherGate(e){if(!e.watcher||!this.config.runWatcher||!this.config.watchersEnabled?.())return{proceed:!0};let t;if(e.watcher.scriptSha256){if(!this._taskFilesDir)return N.warn(`${this.config.logPrefix} Watcher pin unverifiable for ${e.id} (no task files dir); skipping tick`),{proceed:!1};let n=await PIi(e.id,this._taskFilesDir);if(!n||n.sha!==e.watcher.scriptSha256)return this.writeWatcherHistory(e.id,{kind:"error",ts:Date.now(),error:"watching.js changed since approval \u2014 re-run start_watching to re-approve"}),{proceed:!1};t=n.content}let n;try{n=await this.config.runWatcher(e,t)}catch(t){let n=t instanceof Error?t.name:typeof t;return N.error(`${this.config.logPrefix} Watcher tick threw for ${e.id}: ${n}`),this.writeWatcherHistory(e.id,{kind:"error",ts:Date.now(),error:`infra: ${n}`}),{proceed:!1}}let r={durationMs:n.durationMs??0,calls:n.calls??[]};if("error"in n)return N.warn(`${this.config.logPrefix} Watcher tick error for ${e.id}: ${n.error}`),this.writeWatcherHistory(e.id,{kind:"error",ts:Date.now(),...r,error:n.error}),{proceed:!1};let i=kFi(n.signal),a=e.watcher?.fireThreshold??.7,o=Number.isFinite(i)&&i>=a;return N.info(`${this.config.logPrefix} Watcher for ${e.id}: signal=${i} threshold=${a} proceed=${o}`),this.writeWatcherHistory(e.id,{kind:"tick",ts:Date.now(),signal:i,proceed:o,summary:n.summary?.slice(0,200),...r}),{proceed:o,watcherReport:n.summary?.slice(0,EFi),cursors:n.cursors}}
```

## E11 — 卡住的定时运行及并发位置清理

### reapTimedOutScheduledRun

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[463150, 464133)`。

```javascript
reapTimedOutScheduledRun(e,n){if(!e.scheduledTaskId)return;let r=this.isScheduledTaskReapEnabled(),i=r&&this.isScheduledTaskReapDenyEnabled()?this.permissionRouter.denyPendingPermissionsForSession(e.sessionId,rl):0;if(t.lF("lam_scheduled_task_hung_run_reap",{scheduled_task_id:e.scheduledTaskId,session_id:e.sessionId,seconds_since_activity:Math.round(n/1e3),reap_enabled:r,action:r?i>0?"declined":"stopped":"skipped"}),r){if(i>0){t.p$.warn(`[Lifecycle] Declined ${i} unanswered approval(s) on scheduled-task run ${e.sessionId} \u2014 letting it finish`),e._scheduledRunApprovalsDeclined=!0,e._reapShieldAt=Date.now(),this.healthMonitor.clearTimeout(e.sessionId),e._observedRunTools&&(e._observedRunTools.approvalDeclined=!0);return}t.p$.warn(`[Lifecycle] Stopping hung scheduled-task run ${e.sessionId} \u2014 watchdog released its concurrency slot`),this.stopSession(e.sessionId,!0).catch((n=>{t.p$.error(`[Lifecycle] Failed to stop hung scheduled-task run ${e.sessionId}:`,n)}))}}
```

### isScheduledTaskReapEnabled

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[462955, 463052)`。

```javascript
isScheduledTaskReapEnabled(){return t.iG("1978029737","scheduledTaskStaleReapEnabled",!0,t.h1())}
```

### isScheduledTaskReapDenyEnabled

来源：`index.chunk-cz5TcuPg.js`，第 1 行，UTF-16 偏移 `[463052, 463150)`。

```javascript
isScheduledTaskReapDenyEnabled(){return t.iG("1978029737","scheduledTaskStaleReapDeny",!0,t.h1())}
```

## E12 — 页面单次信息综合提示词与调用参数

### $Bi

来源：`index.chunk-BcYASoX4.js`，第 13 行，UTF-16 偏移 `[5805566, 5806405)`。

```javascript
function $Bi(){return`<application_details>\nClaude is powering a Cowork dashboard's synthesis call. A dashboard artifact called window.cowork.askClaude() with a fixed task instruction and fresh data blocks (typically MCP tool results). Claude's job is to produce the requested synthesis \u2014 a summary, classification, extraction, or similar transformation of the provided data.\n\nBe concise. Output only the requested content. The output renders directly inside a dashboard widget, so skip preambles like "Here's the summary:" and get straight to the answer.\n\nThis is a single-turn, tool-free call. Claude cannot ask clarifying questions or use any tools. If the instruction is ambiguous, make a reasonable interpretation and proceed.\n</application_details>\n\n<env>\nToday's date: ${(new Date).toISOString().slice(0,10)}\n</env>`}
```

### vHi

来源：`index.chunk-BcYASoX4.js`，第 13 行，UTF-16 偏移 `[5821372, 5821437)`。

```javascript
function vHi(){return{tags:"artifact_sample",systemPrompt:$Bi()}}
```

## E13 — 单次采样限制、模型选择、撤销与缓存

### p

来源：`index.chunk-XrVlDENe.js`，第 1 行，UTF-16 偏移 `[2642, 5394)`。

```javascript
async function p(e,n,o){if(o.signal?.aborted)return{text:"Inference cancelled.",isError:!0};let c=t.ht();if(c)return t.p$.info(`[TokenCap] one-shot sample refused: in-memory window trip (${c.used}/${c.cap})`),{text:t.pt(c),isError:!0};let l=await t.gt();if(l.over)return t.p$.info(`[TokenCap] one-shot sample refused: persisted window over cap (${l.used}/${l.cap})`),{text:t.pt(l),isError:!0};g();let u=new AbortController;m.add(u);let d=()=>u.abort();o.signal?.addEventListener("abort",d,{once:!0});let p=()=>{m.delete(u),o.signal?.removeEventListener("abort",d)},h;try{let c=t.XF(),l=await t.JF(c);if(!l.ok)return t.p$.warn("[OneShotSampler] OAuth failed %s",t.QH(l.reason)),{text:"Not signed in.",isError:!0};let d=f(e,n),p=t.uZ(),m=p.discoveredRendererConfig(),g=m?t.CU(await m):[],_=p.resolveDefaultSessionModel(),v=t.xO()??t.r_(g)??(_.ok?_.model:void 0)??(p.getProvider()===null?"claude-haiku-4-5-20251001":void 0),{query:y}=await t.zN(),b;try{b=await s({logPrefix:"[OneShotSampler]",label:"sampler",apiHost:c.apiHost,hostComposedEnv:{...t.g_({oauthToken:l.token,apiHost:c.apiHost,disableCron:!0,localAgent:!0}),CLAUDE_CODE_ENTRYPOINT:"local-agent",CLAUDE_CODE_TAGS:o.tags,...await t.j_(void 0,{target:"vm",sandboxed:!1,platformLacksGrpcCaBridge:!0,appVersion:a.app.getVersion()}),CLAUDE_CODE_DISABLE_ATTACHMENTS:"1"}})}catch(e){return t.EX(e)?(t.p$.warn("[OneShotSampler] 3P credential unavailable: %s",t._Y(e)),{text:"Not signed in.",isError:!0}):(t.p$.error("[OneShotSampler] spawn setup failed: %s",t._Y(e)),{text:"Inference failed.",isError:!0})}if(!b.ok)return{text:b.text,isError:!0};if(h=b.dispose,o.signal?.aborted||u.signal.aborted)return{text:"Inference cancelled.",isError:!0};let x={abortController:u,...b.sdk,model:v,maxTurns:1,systemPrompt:o.systemPrompt,...r.t([]),allowedTools:[],settingSources:[],persistSession:!1,canUseTool:async()=>({behavior:"deny",message:"one-shot sampling has no tools"}),stderr:e=>{t.p$.warn(`[OneShotSampler] stderr: ${t.gY(e)}`)}};i.t(x);let S=y({prompt:d,options:x}),C="",w=!1;try{for await(let e of S){if(e.type==="assistant"&&e.message.model!=="<synthetic>")for(let t of e.message.content)t.type==="text"&&(C+=t.text);if(e.type==="result"){if(e.subtype!=="success"||e.is_error)return t.p$.warn("[OneShotSampler] Result %s: %s",e.subtype,t._Y(e)),{text:"Inference failed.",isError:!0};w=!0;break}}}catch(e){return u.signal.aborted?(t.p$.info("[OneShotSampler] query() aborted by caller"),{text:"Inference cancelled.",isError:!0}):(t.p$.error("[OneShotSampler] query() iteration threw: %s",t._Y(e)),{text:"Inference failed.",isError:!0})}return w?{text:t.s_(C)}:(t.p$.warn("[OneShotSampler] query() ended without a result message"),{text:"Inference did not complete.",isError:!0})}finally{h?.(),p()}}
```

### f

来源：`index.chunk-XrVlDENe.js`，第 1 行，UTF-16 偏移 `[2457, 2642)`。

```javascript
function f(e,n){let r=t.o_(e);return!n||n.length===0?d+r:`${d}${n.map((e=>typeof e=="string"?e:JSON.stringify(e)??"undefined")).map((e=>t.o_(`<data>${e}</data>`))).join(`\n`)}\n\n${r}`}
```

### credential revocation cancels in-flight sampling

来源：`index.chunk-XrVlDENe.js`，第 1 行，UTF-16 偏移 `[5394, 5502)`。

```javascript
var m=new Set,h=!1;function g(){h||=(t.uZ().onCredentialRevoked?.((()=>{for(let e of[...m])e.abort()})),!0)}
```

### askClaude artifact cache and dispatch

来源：`index.chunk-BcYASoX4.js`，第 13 行，UTF-16 偏移 `[5829080, 5830988)`。

```javascript
async askClaude(t,n){let r=Date.now(),i=e(),a=e=>F4({method:"ask_claude",host:i,outcome:e,startedAt:r});if(!i)return a("rejected_not_shown"),I4("Artifact is not currently shown.");if(i.kind==="task"){await AHi();let e;try{let{sampleOneShot:r}=await Promise.resolve().then((()=>require("./index.chunk-XrVlDENe.js"))).then((e=>e.t));e=await r(t,n,vHi())}catch(t){e=I4(t instanceof Error?t.message:"Inference failed.")}finally{jHi()}return a(e.isError?"error":"success"),SHi(i.calls,{tool:"askClaude",ms:Date.now()-r,ok:!e.isError,promptPreview:String(t).slice(0,200),answerPreview:e.text?.slice(0,200)}),e}if(!Ab("2940196192"))return a("rejected_disabled"),I4("Artifact inference is not enabled.");let o=i.artifactId,s=NBi("askClaude",t,n),c=await IBi(o,s,GR()),l=e=>x4(o,(()=>({kind:"sample",argsPreview:t,...e})));if(c)return N.info(`[CoworkArtifacts] askClaude() cache hit (artifactId=${o} cacheKey=${s})`),l({durationMs:0,isError:c.isError===!0,resultPreview:c.text,cached:!0}),a("cache_hit"),c;N.info(`[CoworkArtifacts] askClaude() cache miss (artifactId=${o} cacheKey=${s})`);let u=Date.now();await AHi();let d=Date.now();try{if(P4()!==o)return l({durationMs:Date.now()-u,isError:!0,resultPreview:"Artifact is no longer shown."}),a("rejected_not_shown"),I4("Artifact is no longer shown.");let{sampleOneShot:e}=await Promise.resolve().then((()=>require("./index.chunk-XrVlDENe.js"))).then((e=>e.t)),r=await e(t,n,vHi()),i=Date.now()-d;return N.info(`[CoworkArtifacts] askClaude() completed (artifactId=${o} cacheKey=${s} durationMs=${i} isError=${r.isError})`),l({durationMs:i,isError:r.isError===!0,resultPreview:r.text}),a(r.isError?"error":"success"),r.isError!==!0&&BBi(o,s,r),r}catch(e){return N.error("[CoworkArtifacts] askClaude() failed %o",{artifactId:o,err:e}),l({durationMs:Date.now()-d,isError:!0,resultPreview:e}),a("error"),I4(e instanceof Error?e.message:"Inference failed.")}finally{jHi()}
```

### cache expiry, serialized writes and size limits

来源：`index.chunk-BcYASoX4.js`，第 13 行，UTF-16 偏移 `[5802149, 5803197)`。

```javascript
async function IBi(e,t,n){let r=(await PBi(e))[t];if(!(!r||typeof r.updatedAt!="number"||Date.now()-r.updatedAt>=n))return r.value}var LBi=new Map;function RBi(e,t){let n=(LBi.get(e)??Promise.resolve()).then(t,t);return LBi.set(e,n),n.finally((()=>{LBi.get(e)===n&&LBi.delete(e)}))}function zBi(e,t,n){let r=JSON.stringify(n);return r!==void 0&&r.length>jBi?(N.warn("[CoworkArtifacts] Skipping data cache write \u2014 value too large %o",{artifactId:e,key:t,bytes:r.length}),Promise.resolve()):RBi(e,(async()=>{let r=v4.getDataCachePath(e);if(!r||!v4.has(e))return;let i=await PBi(e),a=Date.now(),o=GR();for(let[e,t]of Object.entries(i))(typeof t?.updatedAt!="number"||a-t.updatedAt>=o)&&delete i[e];i[t]={value:n,updatedAt:a};let s=JSON.stringify(i);if(s.length>MBi){N.warn("[CoworkArtifacts] Skipping data cache write \u2014 file too large %o",{artifactId:e,key:t,totalBytes:s.length});return}await di(r,s)}))}function BBi(e,t,n){GR()<=0||zBi(e,t,n).catch((n=>{N.warn("[CoworkArtifacts] Data cache write failed %o",{artifactId:e,key:t,err:n})}))}
```

## E14 — 页面桥接、声明工具范围和手动触发

### preload bridge

来源：`coworkArtifact.js`，第 1 行，UTF-16 偏移 `[0, 2596)`。

```javascript
"use strict";(function(){try{var e=typeof window<"u"?window:typeof global<"u"?global:typeof globalThis<"u"?globalThis:typeof self<"u"?self:{};e.SENTRY_RELEASE={id:"5706e5524dba58b23e105c31c358df8ab0a95852"}}catch{}})();try{(function(){var e=typeof window<"u"?window:typeof global<"u"?global:typeof globalThis<"u"?globalThis:typeof self<"u"?self:{},t=new e.Error().stack;t&&(e._sentryDebugIds=e._sentryDebugIds||{},e._sentryDebugIds[t]="3f3200b9-4a4d-4239-a100-a686e3a2c30a",e._sentryDebugIdIdentifier="sentry-dbid-3f3200b9-4a4d-4239-a100-a686e3a2c30a")})()}catch{}let e=require("electron");require("electron/renderer");var t=function(e){return e.Back="back",e.Forward="forward",e}({}),n="$eipc_message$_f4e2fa98-a0b5-4255-9801-2330dd84a297_$_claude.coworkArtifact_$_",r={callMcpTool(t,r){return e.ipcRenderer.invoke(n+"CoworkArtifactBridge_$_callMcpTool",t,r)},askClaude(t,r){return e.ipcRenderer.invoke(n+"CoworkArtifactBridge_$_askClaude",t,r)},runScheduledTask(t,r){return e.ipcRenderer.invoke(n+"CoworkArtifactBridge_$_runScheduledTask",t,r)},navigateHost(t){return e.ipcRenderer.invoke(n+"CoworkArtifactBridge_$_navigateHost",t)},openExternalUrl(t){return e.ipcRenderer.invoke(n+"CoworkArtifactBridge_$_openExternalUrl",t)},setEditableFocused(t){return e.ipcRenderer.invoke(n+"CoworkArtifactBridge_$_setEditableFocused",t)}};function i(e){let t=e.activeElement;for(;t?.shadowRoot?.activeElement;)t=t.shadowRoot.activeElement;if(!t)return!1;let n=t.tagName;return n==="INPUT"||n==="TEXTAREA"||n==="SELECT"||n==="IFRAME"||n==="FRAME"||n==="EMBED"||n==="OBJECT"||t instanceof HTMLElement&&t.isContentEditable}e.contextBridge.exposeInMainWorld("cowork",{callMcpTool:(e,t)=>r.callMcpTool?.(e,t),askClaude:r.askClaude,runScheduledTask:e=>r.runScheduledTask?.(e,{hasUserActivation:navigator.userActivation.isActive})});function a(e){if(!e.isTrusted)return;let t=e.target?.closest?.("a[href]");t instanceof HTMLAnchorElement&&(!t.protocol||t.protocol==="cowork-artifact:"||(e.preventDefault(),r.openExternalUrl?.(t.href)))}window.addEventListener("click",e=>{e.button===0&&a(e)},!0),window.addEventListener("auxclick",e=>{e.button===1&&a(e)},!0);var o;function s(){let e=i(document);e!==o&&(o=e,r.setEditableFocused?.(e))}window.addEventListener("focusin",s,!0),window.addEventListener("focusout",s,!0),window.addEventListener("keydown",s,!0),s(),process.platform==="darwin"&&window.addEventListener("mouseup",e=>{e.isTrusted&&(e.button===3?(e.preventDefault(),r.navigateHost?.(t.Back)):e.button===4&&(e.preventDefault(),r.navigateHost?.(t.Forward)))},!0);
//# sourceMappingURL=coworkArtifact.js.map
```

### callMcpTool declaration and visibility checks

来源：`index.chunk-BcYASoX4.js`，第 13 行，UTF-16 偏移 `[5830990, 5834484)`。

```javascript
async callMcpTool(t,n){let r=Date.now(),i=e(),a=xHi(t),o=e=>F4({method:"call_mcp_tool",host:i,outcome:e,startedAt:r,toolName:a});if(!i)return o("rejected_not_shown"),yHi("Artifact is not currently shown.");if(i.kind==="task")return zHi(i,t,n);let s=i.artifactId,c=e=>x4(s,(()=>({kind:"callMcpTool",tool:t,argsPreview:n,...e}))),l=(e,t)=>(o(e),c({durationMs:0,isError:!0,resultPreview:t}),yHi(t));if(n!==void 0&&(typeof n!="object"||!n||Array.isArray(n)))return l("rejected_invalid","Tool args must be an object.");let u=v4.getMcpTools(s);if(!Array.isArray(u)||!u.includes(t))return N.warn("[CoworkArtifacts] Bridge call to disallowed tool %o",{artifactId:s,tool:String(t).slice(0,200)}),v4.noteRequestedMcpTool(s,String(t)),l("rejected_allowlist",`Tool "${t}" is not in this artifact's mcp_tools allowlist.`);let d=(await vS()).getMcpCoordinator(),f=v4.getMcpToolServers(s)?.[u.indexOf(t)],p=typeof f=="string"&&f.length>0?f:void 0,m=p?g4(t):null,h=p&&m?{ok:!0,serverKey:p,toolName:m.toolName}:await Ozi(d,t);if(!h.ok)return N.warn(`[CoworkArtifacts] callMcpTool() refused (artifactId=${s} tool=${t}): ${h.reason}`),l("rejected_invalid",h.reason);let{serverKey:g,toolName:_}=h,v=d.isRemoteToolReadOnly(g,_)===!0,y=d.isRemoteToolDestructive(g,_)===!0,b=await d.getManagedToolPolicy(g,_),x=b==="ask",S=bHi(d,g,_),C=v&&b!=="ask"&&b!=="blocked"&&!S,w=NBi("mcp",t,n);if(C){let e=await IBi(s,w,GR());if(e)return N.info(`[CoworkArtifacts] callMcpTool() cache hit (artifactId=${s} tool=${t} cacheKey=${w})`),c({durationMs:0,isError:e.isError===!0,resultPreview:CHi(e),resultShape:DHi(e),cached:!0}),o("cache_hit"),e}if(N.info(`[CoworkArtifacts] callMcpTool() cache ${C?"miss":"bypass"} (artifactId=${s} tool=${t} cacheKey=${w} readOnly=${v} policy=${b})`),b==="blocked")return l("rejected_policy",`Tool "${_}" is not permitted: blocked by managed policy.`);if(S)return l("rejected_policy",`Tool "${_}" is not permitted: disabled by the user.`);if(!x&&cXn()&&y){let e=v4.getAccountId(),r=v4.getOrgId();if(rBi(e,r,s,g,_))N.info(`[CoworkArtifacts] callMcpTool() destructive \u2014 remembered allow (artifactId=${s} tool=${t})`);else{N.info(`[CoworkArtifacts] callMcpTool() destructive \u2014 prompting (artifactId=${s} tool=${t})`);let i=v4.get(s),{ok:a,remember:o}=await LHi(s,_,"destructive",n??void 0,nBi());if(!a)return l("rejected_gesture",`Tool "${_}" was not allowed by the user.`);if(P4()!==s)return l("rejected_not_shown","Artifact is no longer shown.");o&&FHi(i,v4.get(s))&&iBi(e,r,s,g,_)}}if(!NHi(s))return N.warn("[CoworkArtifacts] callMcpTool rate-limited %o",{artifactId:s,tool:t}),l("rejected_rate_limit","Artifact MCP rate limit exceeded \u2014 try again shortly.");let T=Date.now();try{let e=await d.callRemoteTool(`cowork-artifact-${s}`,g,_,n??{},(async()=>{let{ok:e}=await LHi(s,_,"managed-ask",n??void 0);return e&&P4()===s}),{origin:"artifact-bridge"}),r=Date.now()-T;return N.info(`[CoworkArtifacts] callMcpTool() completed (artifactId=${s} tool=${t} cacheKey=${w} durationMs=${r} isError=${e.isError})`),c({durationMs:r,isError:e.isError===!0,resultPreview:CHi(e),resultShape:DHi(e)}),o(e.isError?"error":"success"),C&&e.isError!==!0&&BBi(s,w,e),e}catch(e){return N.error("[CoworkArtifacts] Bridge tool call failed %o",{artifactId:s,tool:t,err:e}),c({durationMs:Date.now()-T,isError:!0,resultPreview:e}),o("error"),oHi()&&RF(`Artifact couldn't reach "${_}".`,sv.Error,{messageForLogging:"cowork_artifact_bridge_tool_failed"}),yHi(e instanceof Error?e.message:"Tool call failed.")}},
```

### runScheduledTask user activation and confirmation

来源：`index.chunk-BcYASoX4.js`，第 13 行，UTF-16 偏移 `[5834484, 5835672)`。

```javascript
async runScheduledTask(t,n){let r=Date.now(),i=e(),a=e=>F4({method:"run_scheduled_task",host:i,outcome:e,startedAt:r});if(!dXn())return a("rejected_disabled"),{isError:!0,error:"runScheduledTask is not enabled."};if(!FS())return a("rejected_disabled"),{isError:!0,error:FHt};if(i?.kind!=="artifact")return a("rejected_not_shown"),{isError:!0,error:"Artifact is not currently shown."};let o=i.artifactId;if(n?.hasUserActivation!==!0)return N.warn("[CoworkArtifacts] runScheduledTask() without user activation %o",{artifactId:o,taskId:t}),a("rejected_gesture"),{isError:!0,error:"runScheduledTask requires a user gesture (click/keypress)."};let s=await o4.get(t);if(!s)return a("error"),{isError:!0,error:`Scheduled task "${t}" not found.`};if(!await IHi(o,s.id))return a("rejected_gesture"),{isError:!0,error:"User declined to run the task."};if(P4()!==o)return a("rejected_not_shown"),{isError:!0,error:"Artifact is no longer shown."};let c=await o4.runNow(s.id,{personStarted:!0});return c?(N.info(`[CoworkArtifacts] runScheduledTask() dispatched (artifactId=${o} taskId=${s.id} sessionId=${c})`),a("success"),{sessionId:c}):(a("error"),{isError:!0,error:"Failed to start the task."})}}}
```
