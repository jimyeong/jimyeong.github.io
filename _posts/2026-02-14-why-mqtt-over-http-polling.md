---
layout: post
title: "Why MQTT over HTTP polling for home sensor data"
date: 2026-02-14
summary: "My bathroom had a mould problem. I built a monitoring system. Here's why I chose MQTT over the simpler alternatives."
tags: [mqtt, system-design, ops-check-service]
project: ops-check-service
---

## The problem

My flat has no extractor fan. Every shower fills the bathroom with steam that has nowhere to go. I needed to know when humidity stays dangerously high — not just spikes, but sustained levels that cause mould.

I put a Zigbee humidity sensor in the bathroom and needed a way to get that data to a server that could run alerting logic. Three options on the table:

<div class="decision-box">
  <div class="decision-label">Decision Point</div>
  <p><strong>Option A — HTTP polling:</strong> Sensor pushes to an API every N seconds. Simple. But the sensor sleeps between readings to save battery. Polling means either waking it constantly (killing battery) or accepting stale data.</p>
  <p><strong>Option B — WebSocket:</strong> Persistent connection. Great for browsers, overkill for a sensor that sends one reading every 5 minutes. The overhead of maintaining the connection isn't worth it.</p>
  <p><strong>Option C — MQTT:</strong> Designed for exactly this — low-power devices that publish small messages infrequently. Built-in QoS levels. Tiny packet overhead. Broker handles routing.</p>
</div>

MQTT was the obvious fit. The sensor publishes when it has data. The server subscribes. No wasted cycles, no connection management on the sensor side.

## What I learned about QoS trade-offs

MQTT gives you three QoS levels. I went with QoS 1 (at-least-once delivery) because losing a humidity reading is worse than processing a duplicate. But that means the ingestion layer needs to handle duplicates — which became its own problem worth a separate post.

```javascript
// Each reading gets a deterministic key
const key = `${sensor_id}:${timestamp_day}:${reading_hash}`;
// INSERT ... ON CONFLICT (key) DO NOTHING
```

> If I were building this for a commercial product with hundreds of sensors, I'd reconsider QoS 2 despite the overhead. At my scale (one sensor, one subscriber), QoS 1 + idempotent writes is simpler and good enough.

## The result

The system has been running for 3 months. Mosquitto broker, Zigbee2MQTT bridge, NestJS server. It caught a sustained humidity event that I would have missed — and that catch led to the landlord sending a mould remediation team.

Sometimes the right protocol choice is the boring one.
