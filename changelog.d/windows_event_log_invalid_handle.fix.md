The `windows_event_log` source now recovers when the Windows Event Log service is restarted while Vector runs. All
subscription handles then return `ERROR_INVALID_HANDLE` from `EvtNext`, which matched no recovery branch, so the source
stayed silent until Vector was restarted. Such a handle is now rebuilt from the bookmark, which survives the service
restart, so collection resumes without gap or duplicate.


If the Event Log service is still down when the rebuild is attempted, it is retried every 5 seconds, and an
inherited idle-channel deadline is capped at 5 seconds for an invalid handle, so recovery is not delayed once
the service is back.

authors: pos-ei-don
