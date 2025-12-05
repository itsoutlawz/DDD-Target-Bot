# Sheet Grid Size Fix - Final

## Problem
```
APIError: [400]: Range (ProfilesTarget!A1004) exceeds grid limits. Max rows: 1000
```

The GitHub version had default 1000 rows, but the bot was trying to append beyond row 1000.

## Solution Applied

### 1. Increased Default Rows in `_get_or_create()`
```python
def _get_or_create(self,name,cols=20,rows=10000):  # Changed from 1000 to 10000
```

### 2. Explicit Row Counts for Each Sheet
```python
# ProfilesTarget: 10,000 rows
self.ws=self._get_or_create("ProfilesTarget", cols=len(COLUMN_ORDER), rows=10000)

# Target: 5,000 rows
self.target=self._get_or_create("Target", cols=4, rows=5000)

# Dashboard: 5,000 rows
self.dashboard = self._get_or_create("Dashboard", cols=11, rows=5000)
```

## Sheet Capacity

| Sheet | Rows | Capacity | Notes |
|-------|------|----------|-------|
| ProfilesTarget | 10,000 | ~10,000 profiles | Main data sheet |
| Target | 5,000 | ~5,000 targets | Queue of profiles to scrape |
| Dashboard | 5,000 | ~5,000 runs | Historical run data |

## Expected Performance

- **Per profile**: ~2-3 seconds (with delays)
- **10 profiles**: ~30-40 seconds
- **100 profiles**: ~5-8 minutes
- **1000 profiles**: ~1-2 hours
- **10,000 profiles**: ~10-20 hours

## Verification

Run the bot and verify:
- ✅ No "exceeds grid limits" errors
- ✅ Data appends to end of sheet
- ✅ All profiles processed successfully

## Files Changed

- `Scraper.py` - Lines 315-316, 338, 347

