# erino-taskglitch-assignment
Real-world Task Management Web App that already functions but contains hidden bugs affecting UI, logic, and performance.

## ✅ ALL BUGS FIXED

### BUG 1: Duplicate Task Loading ✅ FIXED
Line 97 useTask.ts - Opportunistic second fetch that duplicated tasks on fast remounts
**Status**: Deleted/Removed

### BUG 2: Undo Snackbar State Management ✅ FIXED  
**Issue**: Undo snackbar didn't clear state when closed, causing phantom undo operations
**Solution**: Added `clearLastDeleted()` function with proper `useCallback` optimization
**Files**: `App.tsx`, `useTasks.ts`, `TasksContext.tsx`

### BUG 3: Unstable Task Sorting ✅ FIXED
**Issue**: Random tie-breaker in sorting caused task list to reshuffle unpredictably  
**Solution**: Replaced `Math.random()` with stable alphabetical sorting by task title
**Files**: `logic.ts` (sortTasks function), `useTasks.ts`

### BUG 4: Dialog Conflicts ✅ FIXED
**Issue**: Clicking delete/edit buttons opened both action dialog AND details dialog due to event bubbling
**Solution**: Added `e.stopPropagation()` to button handlers 
**Files**: `TaskTable.tsx`

### BUG 5: ROI Calculation & Chart Bucketing ✅ FIXED
**Issue 5.1**: `computeROI` allowed divide-by-zero and non-finite values (Infinity/NaN)
**Issue 5.2**: Chart bucketing incorrectly categorized null/NaN ROI values
**Solution**: Proper validation and null handling in both calculation and visualization
**Files**: `logic.ts`, `ChartsDashboard.tsx`

## 🔧 Other Minor Bugs Fixed

### BUG 6: XSS Vulnerability ✅ FIXED
**Issue**: Task notes rendered as HTML using `dangerouslySetInnerHTML`
**Risk**: Script injection through user input
**Solution**: Replaced with safe text rendering
**File**: `TaskTable.tsx`

### BUG 7: Unstable CSV Headers ✅ FIXED
**Issue**: CSV headers derived from first row object keys, causing column order drift
**Solution**: Use predefined stable headers array
**File**: `csv.ts`

### BUG 8: Improper CSV Escaping ✅ FIXED  
**Issue**: Only quoted newlines, ignored commas and quotes causing malformed CSV
**Solution**: Proper RFC 4180 CSV escaping for all special characters
**File**: `csv.ts`
