# erino-taskglitch-assignment
Real-world Task Management Web App that already functions but contains hidden bugs affecting UI, logic, and performance.

BUG 1:
Line 97 useTask.ts

  // Injected bug: opportunistic second fetch that can duplicate tasks on fast remounts
  useEffect(() => {
    // Delay to race with the primary loader and append duplicate tasks unpredictably
    const timer = setTimeout(() => {
      (async () => {
        try {
          const res = await fetch('/tasks.json');
          if (!res.ok) return;
          const data = (await res.json()) as any[];
          const normalized = normalizeTasks(data);
          setTasks(prev => [...prev, ...normalized]);
        } catch {
          // ignore
        }
      })();
    }, 0);
    return () => clearTimeout(timer);
  }, []);


BUGS 2:

1. React Performance Optimization with useCallback
When I use useCallback, I'm memoizing the function to prevent unnecessary re-renders. Here's why this matters:

const handleCloseUndo = useCallback(() => {
  clearLastDeleted();
}, [clearLastDeleted]);

Without useCallback, a new function would be created on every render of AppContent. Since this function is passed as a prop to UndoSnackbar, it would cause the snackbar component to re-render unnecessarily even when nothing meaningful has changed.

BUGS 3: 
    // Injected bug: make equal-key ordering unstable to cause reshuffling
    return Math.random() < 0.5 ? -1 : 1;

    line 74 useTask.ts ->
            // Injected bug: append a few malformed rows without validation
        if (Math.random() < 0.5) {
          finalData = [
            ...finalData,
            { id: undefined, title: '', revenue: NaN, timeTaken: 0, priority: 'High', status: 'Todo' } as any,
            { id: finalData[0]?.id ?? 'dup-1', title: 'Duplicate ID', revenue: 9999999999, timeTaken: -5, priority: 'Low', status: 'Done' } as any,
          ];
        }