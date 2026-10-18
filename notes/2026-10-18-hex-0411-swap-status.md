# 0411 swap status — 2026-10-18 (hex)

**TL;DR:** ECU/emulator side is as far as it goes from the road. Closed-loop Phase 0 done, Phase 1 in flight. The one plant-side piece gating the closed loop is P01-family ECT/IAT thermistor curves in `config/sensor_sets.py`.

## Where the closed-loop stands

- Phase 0 (open loop: P01 observes plant): DONE 2026-10-07.
- Phase 1 (build): IN FLIGHT — `sensor_pack.py`, `plant_shim.py`, dynamic E1CC, cam counters.
- Plant clock rule still holds: plant supplies sensor engineering values + CKP/CMP edge timestamps only; emulator-side sensor pack converts to raw QADC counts; firmware PW/spark outputs convert back to `ActuatorCommands`.
- Design doc: `goals/p01-0411-2156-distributor-engine-conversion/files/PLANT_COUPLING_DESIGN.md` (Hex's workspace).

## Interface facts

- Injector pollables: `Injector_Pulse_Width_Bank_1 @0xFFFFAD2A`, `Bank_2 @0xFFFFAD2C`, `HDW_Injector_Control_Bitfield @0xFFFFE0D6`.
- Trigger-wheel config for the van bin: 4x crank + 1x cam (`WHEEL_4X`: `ckp_teeth=4`, `ckp_missing=0`). Cycle sync comes from the 1X cam, not the crank pattern.
- `carb_distributor.py` meters fuel from `maf_kg_s` — the plant's sensors dict must include MAF.
- AE7A ("Lo_Res_Option") = 1 on the Express van 4L80E bin (lo-res 4x mode); the P0341 cam/crank-correlation diag only runs in that mode.
- 8x-CKP + MAF-delete 2156 bins: checksums repaired, P0101/2/3 → 'No Report'. Trampoline @0x2C17A, toggle @0xFF9CB8. Open: CKP_Degrees @0x8028, DTC thresholds $143B0/$143B2, RAM toggle bench verify.

## Your action item (Ember)

- `config/sensor_sets.py` (from the __4 drop) supports swappable ECU-family thermistor models. The 0411 emulator needs P01-family ECT/IAT curves there before Phase 1's sensor pack can close the loop on real firmware. That's the one plant-side piece gating the closed loop.

## Boundaries (unchanged)

- Drops install over `engine_sim/engine_sim/` verbatim; tests and demo harness stay intact; re-validate after install (72 passed / 5 parked as of __3).
- Never fix the physics, never rewrite the test helpers, never rename the NN-tag registry entry.

## Bench side (for awareness)

- Ryan is on the ALCAN; bench boxes are at home. No bench data until he's back.
- Bench-side deliverables done from the road: bench-sim wiring guide, 7727 signal/log box v2.2 (stimulates crank/cam, fakes analog sensors, captures injector PW + coil trigger), pin-map interrogation OS for the spare 0411 (flash gated on the reflash-stub test — no BDM hardware exists).
- None of this changes the sim contract.
