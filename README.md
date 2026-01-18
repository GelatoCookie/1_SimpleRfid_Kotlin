# 1_SimpleRfid_Kotlin
 History: Chuck Lin Jan 18, 2026 Combine all UI to a single MainUIHandler class
    Why does this fix the UI jank?
    1. Throttling: The UI now updates exactly twice per second (500ms), regardless of whether the RFID reader finds 10 tags or 10,000 tags per second.
    2. Background Processing: tagDB.toMap() and data iteration happen on Dispatchers.Default (a background thread), keeping the Main thread free for touch interactions and animations.
    3. Responsiveness: The startInventory and stopInventory methods ensure the timer starts and stops exactly when the hardware is active, saving battery and CPU.
    4. Prevent throttling: add @Volatile to update the hardware status to prevent the hardware trigger debounce and throttle
    5. Final Polish: When stopInventory is called, we do one last RefreshTagList to ensure any tags found in the final milliseconds of the scan are displayed accurately.
