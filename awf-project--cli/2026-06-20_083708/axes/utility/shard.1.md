[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 1

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/audit/file_writer.go` | 🔴 CRITICAL | 4 | 95% | [details](#internalinfrastructureauditfilewritergo) |
| `internal/interfaces/tui/command.go` | 🔴 CRITICAL | 1 | 95% | [details](#internalinterfacestuicommandgo) |
| `internal/testutil/mocks/mocks.go` | 🟡 NEEDS_REFACTOR | 194 | 95% | [details](#internaltestutilmocksmocksgo) |
| `internal/domain/errors/codes.go` | 🟡 NEEDS_REFACTOR | 54 | 95% | [details](#internaldomainerrorscodesgo) |
| `internal/testutil/builders/builders.go` | 🟡 NEEDS_REFACTOR | 54 | 95% | [details](#internaltestutilbuildersbuildersgo) |
| `internal/interfaces/cli/ui/output.go` | 🟡 NEEDS_REFACTOR | 43 | 95% | [details](#internalinterfacescliuioutputgo) |
| `internal/domain/workflow/validation_errors.go` | 🟡 NEEDS_REFACTOR | 39 | 95% | [details](#internaldomainworkflowvalidationerrorsgo) |
| `pkg/plugin/sdk/sdk.go` | 🟡 NEEDS_REFACTOR | 35 | 95% | [details](#pkgpluginsdksdkgo) |
| `internal/application/execution_service.go` | 🟡 NEEDS_REFACTOR | 29 | 95% | [details](#internalapplicationexecutionservicego) |
| `internal/application/execution_setup.go` | 🟡 NEEDS_REFACTOR | 28 | 95% | [details](#internalapplicationexecutionsetupgo) |

## 🔍 Symbol Details

### `internal/infrastructure/audit/file_writer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `FileAuditTrailWriter` | L22–L25 | 🔴 DEAD | 85% | 0 external importers. Only local use is the compile-time interface assertion at L14 and as NewFileAuditTrailWriter's return type, neither of which constitutes runtime consumption by another package. |
| `NewFileAuditTrailWriter` | L29–L41 | 🔴 DEAD | 95% | 0 importers. No external caller constructs a FileAuditTrailWriter, so the entire type is never instantiated. |
| `Write` | L46–L67 | 🔴 DEAD | 90% | 0 importers. Because NewFileAuditTrailWriter has 0 importers, no instance is ever created externally, making this method unreachable even via the ports.AuditTrailWriter interface. |
| `Close` | L70–L82 | 🔴 DEAD | 90% | 0 importers. Same reasoning as Write — no external construction means no caller can invoke Close. |

### `internal/interfaces/tui/command.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NewCommand` | L34–L53 | 🔴 DEAD | 95% | Exported function with 0 runtime and 0 type-only importers per exhaustive import analysis. No file calls tui.NewCommand() to register it with a parent cobra command. |

### `internal/testutil/mocks/mocks.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `MockWorkflowRepository` | L58–L64 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockWorkflowRepository` | L67–L71 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Load` | L76–L96 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `List` | L100–L114 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ListWithSource` | L119–L133 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Exists` | L137–L147 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AddWorkflow` | L151–L156 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetLoadError` | L160–L165 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetListError` | L169–L174 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetExistsError` | L178–L183 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L187–L195 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockTemplateRepository` | L205–L210 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockTemplateRepository` | L213–L217 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetTemplate` | L221–L235 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ListTemplates` | L239–L253 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Exists` | L257–L263 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AddTemplate` | L267–L272 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetGetError` | L276–L281 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetListError` | L285–L290 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L294–L301 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockStateStore` | L311–L318 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockStateStore` | L321–L325 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Save` | L328–L358 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Load` | L361–L391 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Delete` | L394–L404 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `List` | L407–L424 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetSaveError` | L427–L431 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetLoadError` | L434–L438 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetDeleteError` | L441–L445 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetListError` | L448–L452 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L455–L463 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockCommandExecutor` | L473–L478 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockCommandExecutor` | L481–L486 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Execute` | L489–L514 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetResult` | L519–L522 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetCommandResult` | L525–L529 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetExecuteError` | L532–L536 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetCalls` | L539–L549 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L552–L558 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockLogger` | L568–L572 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LogMessage` | L575–L579 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockLogger` | L582–L588 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Debug` | L591–L603 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Info` | L606–L618 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Warn` | L621–L633 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L636–L648 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithContext` | L652–L666 | 🔴 DEAD | 80% | Exported but imported by 0 files |
| `GetMessages` | L669–L673 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetMessagesByLevel` | L676–L686 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L689–L693 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockHistoryStore` | L703–L706 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockHistoryStore` | L709–L713 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Record` | L716–L721 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `List` | L724–L730 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetStats` | L733–L735 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Cleanup` | L738–L740 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Close` | L743–L745 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockExpressionValidator` | L755–L759 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockExpressionValidator` | L762–L764 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Compile` | L767–L780 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetCompileError` | L783–L788 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetCompileFunc` | L791–L796 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L799–L804 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockExpressionEvaluator` | L814–L822 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockExpressionEvaluator` | L825–L827 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EvaluateBool` | L830–L843 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EvaluateInt` | L846–L859 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetBoolResult` | L862–L868 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetIntResult` | L871–L877 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetEvaluateBoolFunc` | L880–L885 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetEvaluateIntFunc` | L888–L893 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L896–L905 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockPluginManager` | L916–L924 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockPluginManager` | L927–L931 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Discover` | L935–L948 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Load` | L952–L965 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Init` | L969–L982 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Shutdown` | L986–L998 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ShutdownAll` | L1002–L1016 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Get` | L1020–L1026 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `List` | L1030–L1039 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AddPlugin` | L1043–L1059 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetDiscoverFunc` | L1063–L1067 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetLoadFunc` | L1071–L1075 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetInitFunc` | L1079–L1083 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetShutdownFunc` | L1087–L1091 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetShutdownError` | L1095–L1099 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L1103–L1113 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockAgentRegistry` | L1124–L1127 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockAgentRegistry` | L1130–L1134 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Register` | L1139–L1150 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Get` | L1155–L1165 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `List` | L1169–L1179 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Has` | L1183–L1189 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L1193–L1198 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockAgentProvider` | L1215–L1221 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockAgentProvider` | L1224–L1228 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Execute` | L1233–L1247 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ExecuteConversation` | L1252–L1266 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Name` | L1270–L1275 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L1280–L1289 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetExecuteFunc` | L1293–L1298 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetConversationFunc` | L1302–L1307 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetValidateFunc` | L1311–L1316 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L1320–L1327 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockCLIProcess` | L1331–L1337 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockCLIProcess` | L1340–L1344 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Signal` | L1347–L1352 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Wait` | L1355–L1359 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Done` | L1362–L1364 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Close` | L1367–L1369 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetWaitError` | L1372–L1376 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetSignals` | L1379–L1385 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockCLIStartCall` | L1388–L1391 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockCLIExecutor` | L1401–L1409 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockCLICall` | L1412–L1415 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockCLIExecutor` | L1418–L1422 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Run` | L1425–L1439 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetOutput` | L1442–L1447 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetError` | L1450–L1454 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetCalls` | L1457–L1471 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Start` | L1474–L1485 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetStartCalls` | L1488–L1498 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L1501–L1510 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockAuditTrailWriter` | L1521–L1527 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockAuditTrailWriter` | L1530–L1534 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Write` | L1539–L1553 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Close` | L1557–L1568 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetEvents` | L1572–L1579 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetWriteError` | L1583–L1588 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetCloseError` | L1592–L1597 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsClosed` | L1601–L1606 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L1610–L1618 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockPluginStore` | L1623–L1630 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockPluginStore` | L1633–L1637 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Save` | L1641–L1649 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Load` | L1653–L1661 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetState` | L1665–L1669 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ListDisabled` | L1673–L1683 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockPluginConfig` | L1687–L1692 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockPluginConfig` | L1695–L1699 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetEnabled` | L1703–L1716 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsEnabled` | L1721–L1729 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetConfig` | L1733–L1741 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetConfig` | L1745–L1758 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockPluginStateStore` | L1763–L1766 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockPluginStateStore` | L1769–L1778 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `MockUserInputReader` | L1787–L1793 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockUserInputReader` | L1798–L1802 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ReadInput` | L1807–L1834 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetReadError` | L1838–L1842 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetResponses` | L1846–L1851 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AddResponse` | L1855–L1859 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetCallCount` | L1863–L1867 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L1871–L1878 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockSpanRecord` | L1881–L1887 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockSpan` | L1890–L1893 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `End` | L1895–L1899 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetAttribute` | L1901–L1905 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RecordError` | L1907–L1911 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AddEvent` | L1913–L1917 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Record` | L1920–L1924 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockTracer` | L1927–L1930 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockTracer` | L1933–L1935 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Start` | L1937–L1951 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetSpans` | L1954–L1960 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L1963–L1967 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockEventPublisher` | L1971–L1975 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockEventPublisher` | L1978–L1982 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Publish` | L1986–L1994 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Close` | L1998–L2000 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetEvents` | L2004–L2010 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetPublishError` | L2014–L2018 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L2022–L2027 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockSkillRepository` | L2030–L2034 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockSkillRepository` | L2037–L2041 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Load` | L2045–L2064 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LoadFromPath` | L2068–L2087 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetSkill` | L2091–L2095 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetLoadError` | L2099–L2103 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockAgentRoleRepository` | L2107–L2110 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Load` | L2113–L2118 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LoadFromPath` | L2121–L2126 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockOperationCall` | L2129–L2132 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MockOperationProvider` | L2135–L2141 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMockOperationProvider` | L2144–L2148 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetOperation` | L2152–L2157 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ListOperations` | L2161–L2169 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Execute` | L2173–L2184 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AddOperation` | L2191–L2198 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetExecuteFunc` | L2202–L2207 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetExecuteError` | L2211–L2216 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetExecuteCalls` | L2220–L2226 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L2230–L2237 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/errors/codes.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ErrorCode` | L25–L25 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserInputMissingFile` | L31–L31 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserInputInvalidFormat` | L34–L34 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserInputValidationFailed` | L37–L37 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserInputMissingSkill` | L40–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserInputMissingRole` | L43–L43 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeWorkflowParseYAMLSyntax` | L50–L50 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeWorkflowParseUnknownField` | L53–L53 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeWorkflowValidationCycleDetected` | L56–L56 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeWorkflowValidationMissingState` | L59–L59 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeWorkflowValidationInvalidTransition` | L62–L62 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeExecutionCommandFailed` | L69–L69 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeExecutionCommandTimeout` | L72–L72 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeExecutionParallelPartialFailure` | L75–L75 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeExecutionPluginDisabled` | L78–L78 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeExecutionEventDeliveryFailed` | L81–L81 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeExecutionEventCycleDetected` | L84–L84 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeExecutionEventBufferFull` | L87–L87 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeExecutionPluginChecksumMismatch` | L90–L90 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeExecutionPluginBrokerEmitDenied` | L93–L93 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeExecutionPluginStreamSetupFailed` | L96–L96 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeSystemIOReadFailed` | L103–L103 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeSystemIOWriteFailed` | L106–L106 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeSystemIOPermissionDenied` | L109–L109 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserUpgradeVersionNotFound` | L115–L115 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserUpgradeAlreadyLatest` | L118–L118 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserMCPProxyUnknownKey` | L124–L124 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserMCPProxyUnknownPlugin` | L127–L127 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserMCPProxyUnknownOperation` | L130–L130 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserMCPProxyNameCollision` | L133–L133 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserMCPProxyEmptyProxy` | L136–L136 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserMCPProxyUnsupportedProvider` | L139–L139 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserMCPProxyInfiniteLoopGuard` | L142–L142 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserFacadeIdentifierEmpty` | L149–L149 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserFacadeIdentifierMalformed` | L152–L152 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserFacadePackNotFound` | L155–L155 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserFacadeWorkflowNotFound` | L158–L158 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserFacadeSessionClosed` | L161–L161 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserFacadeInputRejected` | L164–L164 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserFacadeDuplicateResponse` | L167–L167 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserACPInvalidPrompt` | L173–L173 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserACPUnsupportedBlock` | L174–L174 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserACPPromptInFlight` | L175–L175 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserACPUnknownSession` | L176–L176 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeUserACPProtocolVersionUnsupported` | L177–L177 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeSystemUpgradeChecksumMismatch` | L183–L183 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeSystemUpgradeBinaryReplaceFailed` | L186–L186 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeSystemUpgradeDownloadFailed` | L189–L189 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorCodeSystemInternalUnmapped` | L196–L196 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Category` | L209–L212 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Subcategory` | L221–L227 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Specific` | L236–L242 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsValid` | L246–L252 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ExitCode` | L261–L275 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/testutil/builders/builders.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ExecutionServiceBuilder` | L21–L33 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewExecutionServiceBuilder` | L37–L47 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithLogger` | L51–L56 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithExecutor` | L60–L65 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithStateStore` | L69–L74 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithWorkflowRepository` | L78–L83 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithAgentRegistry` | L87–L92 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithSkillRepository` | L94–L99 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithEvaluator` | L103–L108 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithValidator` | L112–L117 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithOutputWriters` | L121–L127 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Build` | L132–L208 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowBuilder` | L213–L215 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewWorkflowBuilder` | L222–L236 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithName` | L239–L242 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithDescription` | L245–L248 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithVersion` | L251–L254 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithAuthor` | L257–L260 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithTags` | L263–L266 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithInitial` | L269–L272 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithStep` | L276–L282 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithSteps` | L285–L290 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithInput` | L293–L296 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithEnv` | L299–L302 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Build` | L305–L307 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepBuilder` | L312–L314 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewStepBuilder` | L318–L326 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewCommandStep` | L329–L337 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewParallelStep` | L340–L349 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewTerminalStep` | L352–L360 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithType` | L363–L366 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithDescription` | L369–L372 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithCommand` | L375–L378 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithScriptFile` | L381–L384 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithDir` | L387–L390 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithBranches` | L393–L396 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithStrategy` | L399–L402 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithMaxConcurrent` | L405–L408 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithTimeout` | L411–L414 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithOnSuccess` | L417–L420 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithOnFailure` | L423–L426 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithTransitions` | L429–L432 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithDependsOn` | L435–L438 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithRetry` | L441–L444 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithCapture` | L447–L450 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithContinueOnError` | L453–L456 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithStatus` | L459–L462 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithLoop` | L465–L468 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithMessage` | L472–L475 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithExitCode` | L479–L482 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithOperation` | L485–L489 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithCallWorkflow` | L492–L495 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithAgent` | L498–L501 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Build` | L504–L506 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/interfaces/cli/ui/output.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OutputFormat` | L28–L28 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `FormatText` | L31–L31 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `FormatJSON` | L32–L32 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `FormatTable` | L33–L33 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `FormatQuiet` | L34–L34 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `String` | L37–L50 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `ParseOutputFormat` | L53–L66 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `ErrorResponse` | L69–L73 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WorkflowInfo` | L76–L81 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WorkflowPackInfo` | L84–L90 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `StepInfo` | L93–L102 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `ExecutionInfo` | L105–L114 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `ResumableInfo` | L117–L124 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `RunResult` | L127–L133 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `ValidationResult` | L136–L141 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `InputInfo` | L144–L150 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `StepSummary` | L153–L157 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `ValidationResultTable` | L160–L167 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `PromptInfo` | L170–L176 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `PluginInfo` | L179–L190 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `OperationEntry` | L193–L196 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `CapabilityEntry` | L199–L203 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `ValidatorEntry` | L206–L209 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `OutputWriter` | L265–L272 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `NewOutputWriter` | L275–L284 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `IsJSONFormat` | L287–L289 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `Out` | L292–L294 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteJSON` | L297–L299 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteWorkflows` | L302–L316 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteExecution` | L319–L331 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteRunResult` | L334–L346 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteValidation` | L349–L365 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteValidationTable` | L368–L381 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteError` | L387–L411 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WritePrompts` | L520–L534 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteWorkflowPacks` | L537–L551 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WritePlugins` | L554–L568 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteOperations` | L571–L585 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteCapabilities` | L588–L602 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteStepTypes` | L605–L619 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteValidators` | L622–L636 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteDryRun` | L639–L649 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |
| `WriteResumableList` | L652–L666 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive analysis. |

### `internal/domain/workflow/validation_errors.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ValidationLevel` | L9–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidationLevelError` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidationLevelWarning` | L13–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidationCode` | L28–L28 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrCycleDetected` | L31–L31 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrUnreachableState` | L32–L32 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrInvalidTransition` | L33–L33 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrNoTerminalState` | L34–L34 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrMissingInitialState` | L35–L35 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrUndefinedInput` | L38–L38 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrUndefinedStep` | L39–L39 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrForwardReference` | L40–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrInvalidWorkflowProperty` | L41–L41 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrInvalidStateProperty` | L42–L42 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrInvalidErrorProperty` | L43–L43 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrInvalidContextProperty` | L44–L44 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrInvalidLoopProperty` | L45–L45 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrUnknownReferenceType` | L46–L46 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrErrorRefOutsideErrorHook` | L47–L47 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrUndefinedLoopVariable` | L50–L50 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrCircularWorkflowCall` | L53–L53 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrUndefinedSubworkflow` | L54–L54 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrMaxNestingExceeded` | L55–L55 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrSkillNotFound` | L58–L58 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrSkillMissingSkillMD` | L59–L59 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrSkillEmptyContent` | L60–L60 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrRoleNotFound` | L63–L63 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrRoleMissingAgentsMD` | L64–L64 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrRoleEmptyContent` | L65–L65 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidationError` | L69–L74 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L77–L82 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsError` | L85–L87 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidationResult` | L90–L93 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HasErrors` | L96–L98 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HasWarnings` | L101–L103 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AllIssues` | L106–L111 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AddError` | L114–L121 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AddWarning` | L124–L131 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ToError` | L135–L149 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `pkg/plugin/sdk/sdk.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Plugin` | L40–L45 | 🔴 DEAD | 95% | 0 importers per exhaustive analysis. Public SDK interface with no consumers in this repo. |
| `OperationHandler` | L47–L49 | 🔴 DEAD | 95% | 0 importers. No file in the repo references this interface. |
| `OperationProvider` | L51–L54 | 🔴 DEAD | 95% | 0 importers. No file in the repo references this interface. |
| `OperationSchemaProvider` | L69–L71 | 🔴 DEAD | 95% | 0 importers. Despite detailed doc comment referencing a gRPC bridge, no file in the repo uses this interface. |
| `OperationMeta` | L80–L84 | 🔴 DEAD | 95% | 0 importers. Return type of OperationSchemaProvider.GetOperationSchema, which is itself dead. |
| `InputMeta` | L89–L96 | 🔴 DEAD | 95% | 0 importers. Field type of OperationMeta.Inputs, which is itself dead. |
| `OutputMeta` | L99–L103 | 🔴 DEAD | 95% | 0 importers. Field type of OperationMeta.Outputs, which is itself dead. |
| `BasePlugin` | L106–L109 | 🔴 DEAD | 95% | 0 importers. No plugin embeds this struct anywhere in the repo. |
| `Name` | L111–L113 | 🔴 DEAD | 95% | Method on dead BasePlugin; 0 importers of the parent struct. |
| `Version` | L115–L117 | 🔴 DEAD | 95% | Method on dead BasePlugin; 0 importers of the parent struct. |
| `Init` | L119–L121 | 🔴 DEAD | 95% | Method on dead BasePlugin; 0 importers of the parent struct. |
| `Shutdown` | L123–L125 | 🔴 DEAD | 95% | Method on dead BasePlugin; 0 importers of the parent struct. |
| `Patterns` | L127–L129 | 🔴 DEAD | 95% | Method on dead BasePlugin; 0 importers of the parent struct. |
| `HandleEvent` | L131–L133 | 🔴 DEAD | 95% | Method on dead BasePlugin; 0 importers of the parent struct. |
| `OperationResult` | L135–L140 | 🔴 DEAD | 95% | 0 importers. Returned by NewSuccessResult/NewErrorResult, both of which are also dead. |
| `NewSuccessResult` | L142–L148 | 🔴 DEAD | 95% | 0 importers. Shown in package doc example but not called by any repo file. |
| `NewErrorResult` | L150–L155 | 🔴 DEAD | 95% | 0 importers. No caller in repo. |
| `NewErrorResultf` | L157–L162 | 🔴 DEAD | 95% | 0 importers. No caller in repo; internally invokes formatString which is only reachable through this dead path. |
| `ToMap` | L164–L176 | 🔴 DEAD | 95% | 0 importers. Method on dead OperationResult; no callers in repo. |
| `InputSchema` | L178–L184 | 🔴 DEAD | 95% | 0 importers. Used as return type of dead RequiredInput/OptionalInput helpers. |
| `OperationSchema` | L186–L191 | 🔴 DEAD | 95% | 0 importers. No file constructs or references this struct. |
| `InputTypeString` | L195–L195 | 🔴 DEAD | 95% | 0 importers. Only referenced locally inside dead ValidInputTypes slice and InputMeta comment. |
| `InputTypeInteger` | L196–L196 | 🔴 DEAD | 95% | 0 importers. Only referenced locally inside dead ValidInputTypes slice. |
| `InputTypeBoolean` | L197–L197 | 🔴 DEAD | 95% | 0 importers. Only referenced locally inside dead ValidInputTypes slice. |
| `InputTypeArray` | L198–L198 | 🔴 DEAD | 95% | 0 importers. Only referenced locally inside dead ValidInputTypes slice. |
| `InputTypeObject` | L199–L199 | 🔴 DEAD | 95% | 0 importers. Only referenced locally inside dead ValidInputTypes slice. |
| `IsValidInputType` | L211–L218 | 🔴 DEAD | 95% | 0 importers. No caller in repo. |
| `RequiredInput` | L220–L226 | 🔴 DEAD | 95% | 0 importers. No caller in repo. |
| `OptionalInput` | L228–L235 | 🔴 DEAD | 95% | 0 importers. No caller in repo. |
| `GetString` | L237–L244 | 🔴 DEAD | 95% | 0 importers. Also not called locally; GetStringDefault calls it but that is itself dead. |
| `GetStringDefault` | L246–L252 | 🔴 DEAD | 95% | 0 importers. No caller in repo. |
| `GetInt` | L254–L269 | 🔴 DEAD | 90% | 0 importers. GetIntDefault calls it locally but that is itself dead. |
| `GetIntDefault` | L271–L277 | 🔴 DEAD | 95% | 0 importers. No caller in repo. |
| `GetBool` | L279–L286 | 🔴 DEAD | 95% | 0 importers. GetBoolDefault calls it locally but that is itself dead. |
| `GetBoolDefault` | L288–L294 | 🔴 DEAD | 95% | 0 importers. No caller in repo. |

### `internal/application/execution_service.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SetOutputWriters` | L85–L88 | 🔴 DEAD | 95% | 0 external importers; stdoutWriter/stderrWriter fields never injected by any caller. |
| `SetDisplayRendererFactory` | L94–L96 | 🔴 DEAD | 95% | 0 external importers; displayRendererFactory never injected externally. |
| `SetTemplateService` | L99–L101 | 🔴 DEAD | 95% | 0 external importers; templateSvc never injected by any caller. |
| `SetOperationProvider` | L105–L107 | 🔴 DEAD | 95% | 0 external importers; operationProvider never injected externally. |
| `SetAgentRegistry` | L111–L113 | 🔴 DEAD | 95% | 0 external importers; agentRegistry never injected externally. |
| `SetToolProxyService` | L118–L120 | 🔴 DEAD | 95% | 0 external importers; toolProxy never injected externally. |
| `SetEventPublisher` | L123–L125 | 🔴 DEAD | 95% | 0 external importers; eventPublisher never injected externally. |
| `SetEvaluator` | L129–L131 | 🔴 DEAD | 95% | 0 external importers; evaluator never injected post-construction. |
| `SetConversationManager` | L136–L146 | 🔴 DEAD | 95% | 0 external importers; conversationMgr never injected by any caller. |
| `SetSkillRepository` | L163–L165 | 🔴 DEAD | 95% | 0 external importers; skillRepo never injected externally. |
| `SetAgentRoleRepository` | L167–L169 | 🔴 DEAD | 95% | 0 external importers; agentRoleRepo never injected externally. |
| `SetAWFPaths` | L177–L179 | 🔴 DEAD | 95% | 0 external importers; awfPaths never injected externally. |
| `SetAuditTrailWriter` | L183–L185 | 🔴 DEAD | 95% | 0 external importers; auditTrailWriter never injected externally. |
| `SetRecorder` | L189–L191 | 🔴 DEAD | 95% | 0 external importers; recorder never injected externally. |
| `SetRecorderFactory` | L195–L197 | 🔴 DEAD | 95% | 0 external importers; recorderFactory never injected externally. |
| `SetTranscriptDir` | L201–L203 | 🔴 DEAD | 95% | 0 external importers; transcriptDir never injected externally. |
| `SetAgentOutputNormalizer` | L208–L210 | 🔴 DEAD | 95% | 0 external importers; agentOutputNormalizer never injected externally. |
| `transcriptBaseDir` | L214–L219 | 🔴 DEAD | 65% | Not called anywhere in this file; may exist in another package file but absent from local code. |
| `SetPluginService` | L224–L226 | 🔴 DEAD | 95% | 0 external importers; pluginSvc never injected externally. |
| `SetStepTypeProvider` | L231–L233 | 🔴 DEAD | 95% | 0 external importers; stepTypeProvider never injected externally. |
| `SetPackWorkflowLoader` | L238–L240 | 🔴 DEAD | 95% | 0 external importers; packWorkflowLoader never injected externally. |
| `SetTracer` | L242–L244 | 🔴 DEAD | 95% | 0 external importers; tracer never injected externally. |
| `NewExecutionService` | L369–L390 | 🔴 DEAD | 95% | 0 external importers; no external caller constructs ExecutionService via this factory. |
| `NewExecutionServiceWithEvaluator` | L393–L416 | 🔴 DEAD | 95% | 0 external importers; evaluator-aware constructor never called externally. |
| `RunWithWorkflowAndRunID` | L422–L429 | 🔴 DEAD | 95% | 0 external importers; pre-loaded workflow entry point never called externally. |
| `runWithCallStack` | L656–L664 | 🔴 DEAD | 65% | Not called in this file; comment says used by executeCallWorkflowStep which must be in another package file. |
| `buildLoopDataChain` | L838–L850 | 🔴 DEAD | 65% | Not called in this file; likely used from interpolation_helpers.go in the same package, but absent from local code. |
| `Resume` | L1745–L1817 | 🔴 DEAD | 95% | 0 external importers; resume entry point never invoked externally. |
| `ListResumable` | L1885–L1903 | 🔴 DEAD | 95% | 0 external importers; resumable execution listing never queried externally. |

### `internal/application/execution_setup.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PluginStateChecker` | L70–L72 | 🔴 DEAD | 95% | Exported interface with 0 external importers. Only referenced within this file as a setupConfig field type, but the entire builder is externally dead. |
| `PluginProviders` | L75–L79 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Only referenced as a setupConfig field type within this file. |
| `NotifyConfig` | L82–L84 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Only referenced as a setupConfig field type within this file. |
| `OutputWriterPair` | L87–L90 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Only referenced as a setupConfig field type within this file. |
| `SetupOption` | L93–L93 | 🔴 DEAD | 95% | Exported type with 0 external importers. Used only within this file as the return type of With* functions and as ExecutionSetup.opts element type. |
| `WithNotifyConfig` | L118–L120 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithPluginState` | L123–L125 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithPluginProviders` | L128–L130 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithTracer` | L133–L135 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithAuditWriter` | L138–L140 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithRecorder` | L143–L145 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithRecorderFactory` | L150–L152 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithTranscriptDir` | L156–L158 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithPackContext` | L162–L167 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithOutputWriters` | L170–L174 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithDisplayRendererFactory` | L178–L180 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithUserInputReader` | L183–L185 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithHistoryStore` | L189–L191 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithTemplatePaths` | L194–L196 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithPluginService` | L199–L201 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithEventPublisher` | L204–L206 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithAgentRoleRepository` | L209–L211 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `WithToolProxy` | L217–L221 | 🔴 DEAD | 95% | Exported With* option with 0 external importers and not called within this file. |
| `ExecutionSetup` | L232–L238 | 🔴 DEAD | 95% | Exported struct with 0 external importers. NewExecutionSetup is also dead; the entire builder is unreachable from outside this package. |
| `NewExecutionSetup` | L242–L256 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. No external entry point creates an ExecutionSetup. |
| `SetupResult` | L260–L267 | 🔴 DEAD | 95% | Exported struct returned by Build, which itself has 0 external importers. Never consumed outside this package. |
| `Build` | L271–L407 | 🔴 DEAD | 90% | Exported method with 0 external importers. The entire ExecutionSetup builder chain is unreachable from outside this package. |
| `MergeInputs` | L507–L512 | 🔴 DEAD | 95% | Exported utility function with 0 external importers and not called within this file. |

## ⚡ Quick Wins

- [ ] <!-- ACT-601abf-1 --> **[utility · high · trivial]** `internal/application/execution_service.go`: Remove dead code: `SetOutputWriters` is exported but unused `SetOutputWriters`, `SetDisplayRendererFactory`, `SetTemplateService`, `SetOperationProvider`, `SetAgentRegistry`, `SetToolProxyService`, `SetEventPublisher`, `SetEvaluator`, `SetConversationManager`, `SetSkillRepository`, `SetAgentRoleRepository`, `SetAWFPaths`, `SetAuditTrailWriter`, `SetRecorder`, `SetRecorderFactory`, `SetTranscriptDir`, `SetAgentOutputNormalizer`, `SetPluginService`, `SetStepTypeProvider`, `SetPackWorkflowLoader`, `SetTracer`, `NewExecutionService`, `NewExecutionServiceWithEvaluator`, `RunWithWorkflowAndRunID`, `Resume`, `ListResumable` (`SetOutputWriters, SetDisplayRendererFactory, SetTemplateService, SetOperationProvider, SetAgentRegistry, SetToolProxyService, SetEventPublisher, SetEvaluator, SetConversationManager, SetSkillRepository, SetAgentRoleRepository, SetAWFPaths, SetAuditTrailWriter, SetRecorder, SetRecorderFactory, SetTranscriptDir, SetAgentOutputNormalizer, SetPluginService, SetStepTypeProvider, SetPackWorkflowLoader, SetTracer, NewExecutionService, NewExecutionServiceWithEvaluator, RunWithWorkflowAndRunID, Resume, ListResumable`) [L85-L88, L94-L96, L99-L101, L105-L107, L111-L113, L118-L120, L123-L125, L129-L131, L136-L146, L163-L165, L167-L169, L177-L179, L183-L185, L189-L191, L195-L197, L201-L203, L208-L210, L224-L226, L231-L233, L238-L240, L242-L244, L369-L390, L393-L416, L422-L429, L1745-L1817, L1885-L1903]
- [ ] <!-- ACT-e9a3fb-1 --> **[utility · high · trivial]** `internal/application/execution_setup.go`: Remove dead code: `PluginStateChecker` is exported but unused `PluginStateChecker`, `PluginProviders`, `NotifyConfig`, `OutputWriterPair`, `SetupOption`, `WithNotifyConfig`, `WithPluginState`, `WithPluginProviders`, `WithTracer`, `WithAuditWriter`, `WithRecorder`, `WithRecorderFactory`, `WithTranscriptDir`, `WithPackContext`, `WithOutputWriters`, `WithDisplayRendererFactory`, `WithUserInputReader`, `WithHistoryStore`, `WithTemplatePaths`, `WithPluginService`, `WithEventPublisher`, `WithAgentRoleRepository`, `WithToolProxy`, `ExecutionSetup`, `NewExecutionSetup`, `SetupResult`, `Build`, `MergeInputs` (`PluginStateChecker, PluginProviders, NotifyConfig, OutputWriterPair, SetupOption, WithNotifyConfig, WithPluginState, WithPluginProviders, WithTracer, WithAuditWriter, WithRecorder, WithRecorderFactory, WithTranscriptDir, WithPackContext, WithOutputWriters, WithDisplayRendererFactory, WithUserInputReader, WithHistoryStore, WithTemplatePaths, WithPluginService, WithEventPublisher, WithAgentRoleRepository, WithToolProxy, ExecutionSetup, NewExecutionSetup, SetupResult, Build, MergeInputs`) [L70-L72, L75-L79, L82-L84, L87-L90, L93-L93, L118-L120, L123-L125, L128-L130, L133-L135, L138-L140, L143-L145, L150-L152, L156-L158, L162-L167, L170-L174, L178-L180, L183-L185, L189-L191, L194-L196, L199-L201, L204-L206, L209-L211, L217-L221, L232-L238, L242-L256, L260-L267, L271-L407, L507-L512]
- [ ] <!-- ACT-808fdf-1 --> **[utility · high · trivial]** `internal/domain/errors/codes.go`: Remove dead code: `ErrorCode` is exported but unused `ErrorCode`, `ErrorCodeUserInputMissingFile`, `ErrorCodeUserInputInvalidFormat`, `ErrorCodeUserInputValidationFailed`, `ErrorCodeUserInputMissingSkill`, `ErrorCodeUserInputMissingRole`, `ErrorCodeWorkflowParseYAMLSyntax`, `ErrorCodeWorkflowParseUnknownField`, `ErrorCodeWorkflowValidationCycleDetected`, `ErrorCodeWorkflowValidationMissingState`, `ErrorCodeWorkflowValidationInvalidTransition`, `ErrorCodeExecutionCommandFailed`, `ErrorCodeExecutionCommandTimeout`, `ErrorCodeExecutionParallelPartialFailure`, `ErrorCodeExecutionPluginDisabled`, `ErrorCodeExecutionEventDeliveryFailed`, `ErrorCodeExecutionEventCycleDetected`, `ErrorCodeExecutionEventBufferFull`, `ErrorCodeExecutionPluginChecksumMismatch`, `ErrorCodeExecutionPluginBrokerEmitDenied`, `ErrorCodeExecutionPluginStreamSetupFailed`, `ErrorCodeSystemIOReadFailed`, `ErrorCodeSystemIOWriteFailed`, `ErrorCodeSystemIOPermissionDenied`, `ErrorCodeUserUpgradeVersionNotFound`, `ErrorCodeUserUpgradeAlreadyLatest`, `ErrorCodeUserMCPProxyUnknownKey`, `ErrorCodeUserMCPProxyUnknownPlugin`, `ErrorCodeUserMCPProxyUnknownOperation`, `ErrorCodeUserMCPProxyNameCollision`, `ErrorCodeUserMCPProxyEmptyProxy`, `ErrorCodeUserMCPProxyUnsupportedProvider`, `ErrorCodeUserMCPProxyInfiniteLoopGuard`, `ErrorCodeUserFacadeIdentifierEmpty`, `ErrorCodeUserFacadeIdentifierMalformed`, `ErrorCodeUserFacadePackNotFound`, `ErrorCodeUserFacadeWorkflowNotFound`, `ErrorCodeUserFacadeSessionClosed`, `ErrorCodeUserFacadeInputRejected`, `ErrorCodeUserFacadeDuplicateResponse`, `ErrorCodeUserACPInvalidPrompt`, `ErrorCodeUserACPUnsupportedBlock`, `ErrorCodeUserACPPromptInFlight`, `ErrorCodeUserACPUnknownSession`, `ErrorCodeUserACPProtocolVersionUnsupported`, `ErrorCodeSystemUpgradeChecksumMismatch`, `ErrorCodeSystemUpgradeBinaryReplaceFailed`, `ErrorCodeSystemUpgradeDownloadFailed`, `ErrorCodeSystemInternalUnmapped`, `Category`, `Subcategory`, `Specific`, `IsValid`, `ExitCode` (`ErrorCode, ErrorCodeUserInputMissingFile, ErrorCodeUserInputInvalidFormat, ErrorCodeUserInputValidationFailed, ErrorCodeUserInputMissingSkill, ErrorCodeUserInputMissingRole, ErrorCodeWorkflowParseYAMLSyntax, ErrorCodeWorkflowParseUnknownField, ErrorCodeWorkflowValidationCycleDetected, ErrorCodeWorkflowValidationMissingState, ErrorCodeWorkflowValidationInvalidTransition, ErrorCodeExecutionCommandFailed, ErrorCodeExecutionCommandTimeout, ErrorCodeExecutionParallelPartialFailure, ErrorCodeExecutionPluginDisabled, ErrorCodeExecutionEventDeliveryFailed, ErrorCodeExecutionEventCycleDetected, ErrorCodeExecutionEventBufferFull, ErrorCodeExecutionPluginChecksumMismatch, ErrorCodeExecutionPluginBrokerEmitDenied, ErrorCodeExecutionPluginStreamSetupFailed, ErrorCodeSystemIOReadFailed, ErrorCodeSystemIOWriteFailed, ErrorCodeSystemIOPermissionDenied, ErrorCodeUserUpgradeVersionNotFound, ErrorCodeUserUpgradeAlreadyLatest, ErrorCodeUserMCPProxyUnknownKey, ErrorCodeUserMCPProxyUnknownPlugin, ErrorCodeUserMCPProxyUnknownOperation, ErrorCodeUserMCPProxyNameCollision, ErrorCodeUserMCPProxyEmptyProxy, ErrorCodeUserMCPProxyUnsupportedProvider, ErrorCodeUserMCPProxyInfiniteLoopGuard, ErrorCodeUserFacadeIdentifierEmpty, ErrorCodeUserFacadeIdentifierMalformed, ErrorCodeUserFacadePackNotFound, ErrorCodeUserFacadeWorkflowNotFound, ErrorCodeUserFacadeSessionClosed, ErrorCodeUserFacadeInputRejected, ErrorCodeUserFacadeDuplicateResponse, ErrorCodeUserACPInvalidPrompt, ErrorCodeUserACPUnsupportedBlock, ErrorCodeUserACPPromptInFlight, ErrorCodeUserACPUnknownSession, ErrorCodeUserACPProtocolVersionUnsupported, ErrorCodeSystemUpgradeChecksumMismatch, ErrorCodeSystemUpgradeBinaryReplaceFailed, ErrorCodeSystemUpgradeDownloadFailed, ErrorCodeSystemInternalUnmapped, Category, Subcategory, Specific, IsValid, ExitCode`) [L25-L25, L31-L31, L34-L34, L37-L37, L40-L40, L43-L43, L50-L50, L53-L53, L56-L56, L59-L59, L62-L62, L69-L69, L72-L72, L75-L75, L78-L78, L81-L81, L84-L84, L87-L87, L90-L90, L93-L93, L96-L96, L103-L103, L106-L106, L109-L109, L115-L115, L118-L118, L124-L124, L127-L127, L130-L130, L133-L133, L136-L136, L139-L139, L142-L142, L149-L149, L152-L152, L155-L155, L158-L158, L161-L161, L164-L164, L167-L167, L173-L173, L174-L174, L175-L175, L176-L176, L177-L177, L183-L183, L186-L186, L189-L189, L196-L196, L209-L212, L221-L227, L236-L242, L246-L252, L261-L275]
- [ ] <!-- ACT-228ae3-1 --> **[utility · high · trivial]** `internal/domain/workflow/validation_errors.go`: Remove dead code: `ValidationLevel` is exported but unused `ValidationLevel`, `ValidationLevelError`, `ValidationLevelWarning`, `ValidationCode`, `ErrCycleDetected`, `ErrUnreachableState`, `ErrInvalidTransition`, `ErrNoTerminalState`, `ErrMissingInitialState`, `ErrUndefinedInput`, `ErrUndefinedStep`, `ErrForwardReference`, `ErrInvalidWorkflowProperty`, `ErrInvalidStateProperty`, `ErrInvalidErrorProperty`, `ErrInvalidContextProperty`, `ErrInvalidLoopProperty`, `ErrUnknownReferenceType`, `ErrErrorRefOutsideErrorHook`, `ErrUndefinedLoopVariable`, `ErrCircularWorkflowCall`, `ErrUndefinedSubworkflow`, `ErrMaxNestingExceeded`, `ErrSkillNotFound`, `ErrSkillMissingSkillMD`, `ErrSkillEmptyContent`, `ErrRoleNotFound`, `ErrRoleMissingAgentsMD`, `ErrRoleEmptyContent`, `ValidationError`, `Error`, `IsError`, `ValidationResult`, `HasErrors`, `HasWarnings`, `AllIssues`, `AddError`, `AddWarning`, `ToError` (`ValidationLevel, ValidationLevelError, ValidationLevelWarning, ValidationCode, ErrCycleDetected, ErrUnreachableState, ErrInvalidTransition, ErrNoTerminalState, ErrMissingInitialState, ErrUndefinedInput, ErrUndefinedStep, ErrForwardReference, ErrInvalidWorkflowProperty, ErrInvalidStateProperty, ErrInvalidErrorProperty, ErrInvalidContextProperty, ErrInvalidLoopProperty, ErrUnknownReferenceType, ErrErrorRefOutsideErrorHook, ErrUndefinedLoopVariable, ErrCircularWorkflowCall, ErrUndefinedSubworkflow, ErrMaxNestingExceeded, ErrSkillNotFound, ErrSkillMissingSkillMD, ErrSkillEmptyContent, ErrRoleNotFound, ErrRoleMissingAgentsMD, ErrRoleEmptyContent, ValidationError, Error, IsError, ValidationResult, HasErrors, HasWarnings, AllIssues, AddError, AddWarning, ToError`) [L9-L9, L12-L12, L13-L13, L28-L28, L31-L31, L32-L32, L33-L33, L34-L34, L35-L35, L38-L38, L39-L39, L40-L40, L41-L41, L42-L42, L43-L43, L44-L44, L45-L45, L46-L46, L47-L47, L50-L50, L53-L53, L54-L54, L55-L55, L58-L58, L59-L59, L60-L60, L63-L63, L64-L64, L65-L65, L69-L74, L77-L82, L85-L87, L90-L93, L96-L98, L101-L103, L106-L111, L114-L121, L124-L131, L135-L149]
- [ ] <!-- ACT-1ab882-4 --> **[utility · high · trivial]** `internal/infrastructure/audit/file_writer.go`: Remove dead code: `FileAuditTrailWriter` is exported but unused `FileAuditTrailWriter`, `NewFileAuditTrailWriter`, `Write`, `Close` (`FileAuditTrailWriter, NewFileAuditTrailWriter, Write, Close`) [L22-L25, L29-L41, L46-L67, L70-L82]
- [ ] <!-- ACT-de0f12-2 --> **[utility · high · trivial]** `internal/interfaces/cli/ui/output.go`: Remove dead code: `OutputFormat` is exported but unused `OutputFormat`, `FormatText`, `FormatJSON`, `FormatTable`, `FormatQuiet`, `String`, `ParseOutputFormat`, `ErrorResponse`, `WorkflowInfo`, `WorkflowPackInfo`, `StepInfo`, `ExecutionInfo`, `ResumableInfo`, `RunResult`, `ValidationResult`, `InputInfo`, `StepSummary`, `ValidationResultTable`, `PromptInfo`, `PluginInfo`, `OperationEntry`, `CapabilityEntry`, `ValidatorEntry`, `OutputWriter`, `NewOutputWriter`, `IsJSONFormat`, `Out`, `WriteJSON`, `WriteWorkflows`, `WriteExecution`, `WriteRunResult`, `WriteValidation`, `WriteValidationTable`, `WriteError`, `WritePrompts`, `WriteWorkflowPacks`, `WritePlugins`, `WriteOperations`, `WriteCapabilities`, `WriteStepTypes`, `WriteValidators`, `WriteDryRun`, `WriteResumableList` (`OutputFormat, FormatText, FormatJSON, FormatTable, FormatQuiet, String, ParseOutputFormat, ErrorResponse, WorkflowInfo, WorkflowPackInfo, StepInfo, ExecutionInfo, ResumableInfo, RunResult, ValidationResult, InputInfo, StepSummary, ValidationResultTable, PromptInfo, PluginInfo, OperationEntry, CapabilityEntry, ValidatorEntry, OutputWriter, NewOutputWriter, IsJSONFormat, Out, WriteJSON, WriteWorkflows, WriteExecution, WriteRunResult, WriteValidation, WriteValidationTable, WriteError, WritePrompts, WriteWorkflowPacks, WritePlugins, WriteOperations, WriteCapabilities, WriteStepTypes, WriteValidators, WriteDryRun, WriteResumableList`) [L28-L28, L31-L31, L32-L32, L33-L33, L34-L34, L37-L50, L53-L66, L69-L73, L76-L81, L84-L90, L93-L102, L105-L114, L117-L124, L127-L133, L136-L141, L144-L150, L153-L157, L160-L167, L170-L176, L179-L190, L193-L196, L199-L203, L206-L209, L265-L272, L275-L284, L287-L289, L292-L294, L297-L299, L302-L316, L319-L331, L334-L346, L349-L365, L368-L381, L387-L411, L520-L534, L537-L551, L554-L568, L571-L585, L588-L602, L605-L619, L622-L636, L639-L649, L652-L666]
- [ ] <!-- ACT-3a9c7e-3 --> **[utility · high · trivial]** `internal/interfaces/tui/command.go`: Remove dead code: `NewCommand` is exported but unused (`NewCommand`) [L34-L53]
- [ ] <!-- ACT-89ec69-3 --> **[utility · high · trivial]** `internal/testutil/builders/builders.go`: Remove dead code: `ExecutionServiceBuilder` is exported but unused `ExecutionServiceBuilder`, `NewExecutionServiceBuilder`, `WithLogger`, `WithExecutor`, `WithStateStore`, `WithWorkflowRepository`, `WithAgentRegistry`, `WithSkillRepository`, `WithEvaluator`, `WithValidator`, `WithOutputWriters`, `Build`, `WorkflowBuilder`, `NewWorkflowBuilder`, `WithName`, `WithDescription`, `WithVersion`, `WithAuthor`, `WithTags`, `WithInitial`, `WithStep`, `WithSteps`, `WithInput`, `WithEnv`, `Build`, `StepBuilder`, `NewStepBuilder`, `NewCommandStep`, `NewParallelStep`, `NewTerminalStep`, `WithType`, `WithDescription`, `WithCommand`, `WithScriptFile`, `WithDir`, `WithBranches`, `WithStrategy`, `WithMaxConcurrent`, `WithTimeout`, `WithOnSuccess`, `WithOnFailure`, `WithTransitions`, `WithDependsOn`, `WithRetry`, `WithCapture`, `WithContinueOnError`, `WithStatus`, `WithLoop`, `WithMessage`, `WithExitCode`, `WithOperation`, `WithCallWorkflow`, `WithAgent`, `Build` (`ExecutionServiceBuilder, NewExecutionServiceBuilder, WithLogger, WithExecutor, WithStateStore, WithWorkflowRepository, WithAgentRegistry, WithSkillRepository, WithEvaluator, WithValidator, WithOutputWriters, Build, WorkflowBuilder, NewWorkflowBuilder, WithName, WithDescription, WithVersion, WithAuthor, WithTags, WithInitial, WithStep, WithSteps, WithInput, WithEnv, Build, StepBuilder, NewStepBuilder, NewCommandStep, NewParallelStep, NewTerminalStep, WithType, WithDescription, WithCommand, WithScriptFile, WithDir, WithBranches, WithStrategy, WithMaxConcurrent, WithTimeout, WithOnSuccess, WithOnFailure, WithTransitions, WithDependsOn, WithRetry, WithCapture, WithContinueOnError, WithStatus, WithLoop, WithMessage, WithExitCode, WithOperation, WithCallWorkflow, WithAgent, Build`) [L21-L33, L37-L47, L51-L56, L60-L65, L69-L74, L78-L83, L87-L92, L94-L99, L103-L108, L112-L117, L121-L127, L132-L208, L213-L215, L222-L236, L239-L242, L245-L248, L251-L254, L257-L260, L263-L266, L269-L272, L276-L282, L285-L290, L293-L296, L299-L302, L305-L307, L312-L314, L318-L326, L329-L337, L340-L349, L352-L360, L363-L366, L369-L372, L375-L378, L381-L384, L387-L390, L393-L396, L399-L402, L405-L408, L411-L414, L417-L420, L423-L426, L429-L432, L435-L438, L441-L444, L447-L450, L453-L456, L459-L462, L465-L468, L472-L475, L479-L482, L485-L489, L492-L495, L498-L501, L504-L506]
- [ ] <!-- ACT-d2caf6-3 --> **[utility · high · trivial]** `internal/testutil/mocks/mocks.go`: Remove dead code: `MockWorkflowRepository` is exported but unused `MockWorkflowRepository`, `NewMockWorkflowRepository`, `Load`, `List`, `ListWithSource`, `Exists`, `AddWorkflow`, `SetLoadError`, `SetListError`, `SetExistsError`, `Clear`, `MockTemplateRepository`, `NewMockTemplateRepository`, `GetTemplate`, `ListTemplates`, `Exists`, `AddTemplate`, `SetGetError`, `SetListError`, `Clear`, `MockStateStore`, `NewMockStateStore`, `Save`, `Load`, `Delete`, `List`, `SetSaveError`, `SetLoadError`, `SetDeleteError`, `SetListError`, `Clear`, `MockCommandExecutor`, `NewMockCommandExecutor`, `Execute`, `SetResult`, `SetCommandResult`, `SetExecuteError`, `GetCalls`, `Clear`, `MockLogger`, `LogMessage`, `NewMockLogger`, `Debug`, `Info`, `Warn`, `Error`, `WithContext`, `GetMessages`, `GetMessagesByLevel`, `Clear`, `MockHistoryStore`, `NewMockHistoryStore`, `Record`, `List`, `GetStats`, `Cleanup`, `Close`, `MockExpressionValidator`, `NewMockExpressionValidator`, `Compile`, `SetCompileError`, `SetCompileFunc`, `Clear`, `MockExpressionEvaluator`, `NewMockExpressionEvaluator`, `EvaluateBool`, `EvaluateInt`, `SetBoolResult`, `SetIntResult`, `SetEvaluateBoolFunc`, `SetEvaluateIntFunc`, `Clear`, `MockPluginManager`, `NewMockPluginManager`, `Discover`, `Load`, `Init`, `Shutdown`, `ShutdownAll`, `Get`, `List`, `AddPlugin`, `SetDiscoverFunc`, `SetLoadFunc`, `SetInitFunc`, `SetShutdownFunc`, `SetShutdownError`, `Clear`, `MockAgentRegistry`, `NewMockAgentRegistry`, `Register`, `Get`, `List`, `Has`, `Clear`, `MockAgentProvider`, `NewMockAgentProvider`, `Execute`, `ExecuteConversation`, `Name`, `Validate`, `SetExecuteFunc`, `SetConversationFunc`, `SetValidateFunc`, `Clear`, `MockCLIProcess`, `NewMockCLIProcess`, `Signal`, `Wait`, `Done`, `Close`, `SetWaitError`, `GetSignals`, `MockCLIStartCall`, `MockCLIExecutor`, `MockCLICall`, `NewMockCLIExecutor`, `Run`, `SetOutput`, `SetError`, `GetCalls`, `Start`, `GetStartCalls`, `Clear`, `MockAuditTrailWriter`, `NewMockAuditTrailWriter`, `Write`, `Close`, `GetEvents`, `SetWriteError`, `SetCloseError`, `IsClosed`, `Clear`, `MockPluginStore`, `NewMockPluginStore`, `Save`, `Load`, `GetState`, `ListDisabled`, `MockPluginConfig`, `NewMockPluginConfig`, `SetEnabled`, `IsEnabled`, `GetConfig`, `SetConfig`, `MockPluginStateStore`, `NewMockPluginStateStore`, `MockUserInputReader`, `NewMockUserInputReader`, `ReadInput`, `SetReadError`, `SetResponses`, `AddResponse`, `GetCallCount`, `Clear`, `MockSpanRecord`, `MockSpan`, `End`, `SetAttribute`, `RecordError`, `AddEvent`, `Record`, `MockTracer`, `NewMockTracer`, `Start`, `GetSpans`, `Clear`, `MockEventPublisher`, `NewMockEventPublisher`, `Publish`, `Close`, `GetEvents`, `SetPublishError`, `Clear`, `MockSkillRepository`, `NewMockSkillRepository`, `Load`, `LoadFromPath`, `SetSkill`, `SetLoadError`, `MockAgentRoleRepository`, `Load`, `LoadFromPath`, `MockOperationCall`, `MockOperationProvider`, `NewMockOperationProvider`, `GetOperation`, `ListOperations`, `Execute`, `AddOperation`, `SetExecuteFunc`, `SetExecuteError`, `GetExecuteCalls`, `Clear` (`MockWorkflowRepository, NewMockWorkflowRepository, Load, List, ListWithSource, Exists, AddWorkflow, SetLoadError, SetListError, SetExistsError, Clear, MockTemplateRepository, NewMockTemplateRepository, GetTemplate, ListTemplates, Exists, AddTemplate, SetGetError, SetListError, Clear, MockStateStore, NewMockStateStore, Save, Load, Delete, List, SetSaveError, SetLoadError, SetDeleteError, SetListError, Clear, MockCommandExecutor, NewMockCommandExecutor, Execute, SetResult, SetCommandResult, SetExecuteError, GetCalls, Clear, MockLogger, LogMessage, NewMockLogger, Debug, Info, Warn, Error, WithContext, GetMessages, GetMessagesByLevel, Clear, MockHistoryStore, NewMockHistoryStore, Record, List, GetStats, Cleanup, Close, MockExpressionValidator, NewMockExpressionValidator, Compile, SetCompileError, SetCompileFunc, Clear, MockExpressionEvaluator, NewMockExpressionEvaluator, EvaluateBool, EvaluateInt, SetBoolResult, SetIntResult, SetEvaluateBoolFunc, SetEvaluateIntFunc, Clear, MockPluginManager, NewMockPluginManager, Discover, Load, Init, Shutdown, ShutdownAll, Get, List, AddPlugin, SetDiscoverFunc, SetLoadFunc, SetInitFunc, SetShutdownFunc, SetShutdownError, Clear, MockAgentRegistry, NewMockAgentRegistry, Register, Get, List, Has, Clear, MockAgentProvider, NewMockAgentProvider, Execute, ExecuteConversation, Name, Validate, SetExecuteFunc, SetConversationFunc, SetValidateFunc, Clear, MockCLIProcess, NewMockCLIProcess, Signal, Wait, Done, Close, SetWaitError, GetSignals, MockCLIStartCall, MockCLIExecutor, MockCLICall, NewMockCLIExecutor, Run, SetOutput, SetError, GetCalls, Start, GetStartCalls, Clear, MockAuditTrailWriter, NewMockAuditTrailWriter, Write, Close, GetEvents, SetWriteError, SetCloseError, IsClosed, Clear, MockPluginStore, NewMockPluginStore, Save, Load, GetState, ListDisabled, MockPluginConfig, NewMockPluginConfig, SetEnabled, IsEnabled, GetConfig, SetConfig, MockPluginStateStore, NewMockPluginStateStore, MockUserInputReader, NewMockUserInputReader, ReadInput, SetReadError, SetResponses, AddResponse, GetCallCount, Clear, MockSpanRecord, MockSpan, End, SetAttribute, RecordError, AddEvent, Record, MockTracer, NewMockTracer, Start, GetSpans, Clear, MockEventPublisher, NewMockEventPublisher, Publish, Close, GetEvents, SetPublishError, Clear, MockSkillRepository, NewMockSkillRepository, Load, LoadFromPath, SetSkill, SetLoadError, MockAgentRoleRepository, Load, LoadFromPath, MockOperationCall, MockOperationProvider, NewMockOperationProvider, GetOperation, ListOperations, Execute, AddOperation, SetExecuteFunc, SetExecuteError, GetExecuteCalls, Clear`) [L58-L64, L67-L71, L76-L96, L100-L114, L119-L133, L137-L147, L151-L156, L160-L165, L169-L174, L178-L183, L187-L195, L205-L210, L213-L217, L221-L235, L239-L253, L257-L263, L267-L272, L276-L281, L285-L290, L294-L301, L311-L318, L321-L325, L328-L358, L361-L391, L394-L404, L407-L424, L427-L431, L434-L438, L441-L445, L448-L452, L455-L463, L473-L478, L481-L486, L489-L514, L519-L522, L525-L529, L532-L536, L539-L549, L552-L558, L568-L572, L575-L579, L582-L588, L591-L603, L606-L618, L621-L633, L636-L648, L652-L666, L669-L673, L676-L686, L689-L693, L703-L706, L709-L713, L716-L721, L724-L730, L733-L735, L738-L740, L743-L745, L755-L759, L762-L764, L767-L780, L783-L788, L791-L796, L799-L804, L814-L822, L825-L827, L830-L843, L846-L859, L862-L868, L871-L877, L880-L885, L888-L893, L896-L905, L916-L924, L927-L931, L935-L948, L952-L965, L969-L982, L986-L998, L1002-L1016, L1020-L1026, L1030-L1039, L1043-L1059, L1063-L1067, L1071-L1075, L1079-L1083, L1087-L1091, L1095-L1099, L1103-L1113, L1124-L1127, L1130-L1134, L1139-L1150, L1155-L1165, L1169-L1179, L1183-L1189, L1193-L1198, L1215-L1221, L1224-L1228, L1233-L1247, L1252-L1266, L1270-L1275, L1280-L1289, L1293-L1298, L1302-L1307, L1311-L1316, L1320-L1327, L1331-L1337, L1340-L1344, L1347-L1352, L1355-L1359, L1362-L1364, L1367-L1369, L1372-L1376, L1379-L1385, L1388-L1391, L1401-L1409, L1412-L1415, L1418-L1422, L1425-L1439, L1442-L1447, L1450-L1454, L1457-L1471, L1474-L1485, L1488-L1498, L1501-L1510, L1521-L1527, L1530-L1534, L1539-L1553, L1557-L1568, L1572-L1579, L1583-L1588, L1592-L1597, L1601-L1606, L1610-L1618, L1623-L1630, L1633-L1637, L1641-L1649, L1653-L1661, L1665-L1669, L1673-L1683, L1687-L1692, L1695-L1699, L1703-L1716, L1721-L1729, L1733-L1741, L1745-L1758, L1763-L1766, L1769-L1778, L1787-L1793, L1798-L1802, L1807-L1834, L1838-L1842, L1846-L1851, L1855-L1859, L1863-L1867, L1871-L1878, L1881-L1887, L1890-L1893, L1895-L1899, L1901-L1905, L1907-L1911, L1913-L1917, L1920-L1924, L1927-L1930, L1933-L1935, L1937-L1951, L1954-L1960, L1963-L1967, L1971-L1975, L1978-L1982, L1986-L1994, L1998-L2000, L2004-L2010, L2014-L2018, L2022-L2027, L2030-L2034, L2037-L2041, L2045-L2064, L2068-L2087, L2091-L2095, L2099-L2103, L2107-L2110, L2113-L2118, L2121-L2126, L2129-L2132, L2135-L2141, L2144-L2148, L2152-L2157, L2161-L2169, L2173-L2184, L2191-L2198, L2202-L2207, L2211-L2216, L2220-L2226, L2230-L2237]
- [ ] <!-- ACT-9602d3-3 --> **[utility · high · trivial]** `pkg/plugin/sdk/sdk.go`: Remove dead code: `Plugin` is exported but unused `Plugin`, `OperationHandler`, `OperationProvider`, `OperationSchemaProvider`, `OperationMeta`, `InputMeta`, `OutputMeta`, `BasePlugin`, `Name`, `Version`, `Init`, `Shutdown`, `Patterns`, `HandleEvent`, `OperationResult`, `NewSuccessResult`, `NewErrorResult`, `NewErrorResultf`, `ToMap`, `InputSchema`, `OperationSchema`, `InputTypeString`, `InputTypeInteger`, `InputTypeBoolean`, `InputTypeArray`, `InputTypeObject`, `IsValidInputType`, `RequiredInput`, `OptionalInput`, `GetString`, `GetStringDefault`, `GetInt`, `GetIntDefault`, `GetBool`, `GetBoolDefault` (`Plugin, OperationHandler, OperationProvider, OperationSchemaProvider, OperationMeta, InputMeta, OutputMeta, BasePlugin, Name, Version, Init, Shutdown, Patterns, HandleEvent, OperationResult, NewSuccessResult, NewErrorResult, NewErrorResultf, ToMap, InputSchema, OperationSchema, InputTypeString, InputTypeInteger, InputTypeBoolean, InputTypeArray, InputTypeObject, IsValidInputType, RequiredInput, OptionalInput, GetString, GetStringDefault, GetInt, GetIntDefault, GetBool, GetBoolDefault`) [L40-L45, L47-L49, L51-L54, L69-L71, L80-L84, L89-L96, L99-L103, L106-L109, L111-L113, L115-L117, L119-L121, L123-L125, L127-L129, L131-L133, L135-L140, L142-L148, L150-L155, L157-L162, L164-L176, L178-L184, L186-L191, L195-L195, L196-L196, L197-L197, L198-L198, L199-L199, L211-L218, L220-L226, L228-L235, L237-L244, L246-L252, L254-L269, L271-L277, L279-L286, L288-L294]

## 🔧 Refactors

- [ ] <!-- ACT-601abf-2 --> **[utility · medium · trivial]** `internal/application/execution_service.go`: Remove dead code: `transcriptBaseDir` is exported but unused `transcriptBaseDir`, `runWithCallStack`, `buildLoopDataChain` (`transcriptBaseDir, runWithCallStack, buildLoopDataChain`) [L214-L219, L656-L664, L838-L850]
