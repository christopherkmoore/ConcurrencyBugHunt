![Concurrency Bugs Every iOS Dev Should Know](banner.jpg)

# ConcurrencyBugHunt

An Xcode project full of intentionally buggy Swift code. Each exercise presents real code with a real concurrency problem — your job is to read it, find the bug, and fix it.

Companion to the **[Concurrency Bugs Every iOS Dev Should Know](https://pixelper.com/blog/ios-concurrency-bugs-intro)** series on [pixelper.com](https://pixelper.com/blog).

**Requires:** Xcode 15+, iOS 17+

---

## Getting Started

```bash
git clone https://github.com/christopherkmoore/ConcurrencyBugHunt.git
cd ConcurrencyBugHunt
open ConcurrencyBugHunt.xcodeproj
```

Or regenerate with XcodeGen:
```bash
brew install xcodegen && xcodegen generate
```

---

## Exercises

### GCD

**Race Condition** — A `ShoppingCart` with an unprotected dictionary. Concurrent `addItem()` calls race on reads and writes, producing inconsistent totals. A `DispatchGroup` spawns the concurrent operations so you can watch the results vary between runs.
> [Race Conditions: When Threads Collide](https://pixelper.com/blog/ios-race-conditions) · [Thread-Safe Collections in Swift](https://pixelper.com/blog/ios-thread-safe-collections)

**Main Thread Violation** — A `UserProfileLoader` that fetches data on `DispatchQueue.global()` and then writes directly to `@Published` properties from that background thread. Watch the console for SwiftUI threading warnings.
> [Main Thread Violations: The Silent Crasher](https://pixelper.com/blog/ios-main-thread-violations) · [DispatchQueue.main vs @MainActor](https://pixelper.com/blog/ios-dispatchqueue-vs-mainactor)

**Deadlock** — Two scenarios: a `DataCache` that calls `queue.sync` on the same serial queue it's already running on, and a `BankAccount` with a lock ordering problem where `transfer()` and `logTransaction()` try to acquire the same two locks in opposite order. ⚠️ The demo will freeze the app — it requires a restart.
> [Deadlocks: Sync to Same Queue](https://pixelper.com/blog/ios-deadlocks-sync-queue) · [Lock Ordering and Circular Dependencies](https://pixelper.com/blog/ios-lock-ordering)

---

### Async/Await

**Task Cancellation** — A search bar where each keystroke fires a new `Task` but old tasks aren't cancelled. Type quickly and watch stale results arrive out of order. A `DataProcessor` with no cancellation checks keeps running even after being cancelled.
> [Task Cancellation: The Cooperative Contract](https://pixelper.com/blog/ios-task-cancellation)

**Actor Reentrancy** — A `BankAccountActor` that checks the balance, then awaits a fraud check, then deducts. Because actors allow re-entrance at `await` points, two concurrent withdrawals can both pass the balance check and both succeed — even when there isn't enough money.
> [Actor Reentrancy: State Changes During Await](https://pixelper.com/blog/ios-actor-reentrancy)

**Unstructured Task Leak** — An `ImageLoader` that creates tasks without tracking them, and a `PollingService` with an infinite `while true` loop and no cancellation path. Navigate away and come back — the polling is still running.
> [Unstructured Task Leaks](https://pixelper.com/blog/ios-unstructured-task-leaks) · [Structured vs Unstructured Concurrency](https://pixelper.com/blog/ios-structured-concurrency)

---

### Combine

**Retain Cycle** — A `UserSessionManager` and a `SearchService` that both capture `self` strongly in `sink` closures. Dismiss the sheet and check the console — `deinit` never fires.
> [Combine Retain Cycles: \[weak self\] Matters](https://pixelper.com/blog/ios-combine-retain-cycles)

**Missing Cancellable** — Three examples: a `NotificationListener` that creates subscriptions but doesn't store them (immediately freed), a `DataFetcher` that overwrites its single `AnyCancellable` with each new request, and an `EventSubscriber` where the subscription is lost the moment it's created.
> [The Missing Cancellable Problem](https://pixelper.com/blog/ios-missing-cancellable) · [Managing Multiple Subscriptions](https://pixelper.com/blog/ios-multiple-subscriptions)

---

[pixelper.com](https://pixelper.com) · [Blog](https://pixelper.com/blog)
