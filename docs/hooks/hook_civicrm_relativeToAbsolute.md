# hook_civicrm_relativeToAbsolute

## Summary

This hook is called to alter the from and to of relative date.

## Definition

    hook_civicrm_relativeToAbsolute(&$from, &$to, $relativeTerm, $unit)

## Parameters

-   array $from - reference to list of from date (YmdHis)
-   array $to - reference to list of to date (YmdHis)
-   string $relativeTerm - Relative time frame: this, previous, previous_1.
-   string $unit - Frequency unit like year, month, week etc.

## Returns

## Example

     /**
      * Implements hook_civicrm_relativeToAbsolute().
      *
      * @link http://wiki.civicrm.org/confluence/display/CRMDOC/hook_civicrm_relativeToAbsolute
      */
     function modulename_civicrm_relativeToAbsolute(&$from, &$to, $relativeTerm, $unit) {
       if ($unit == 'quarter' && $relativeTerm == 'this_next') {
         $now = getdate();
         // this quarter
         $quarter = ceil($now['mon'] / 3);
         $from['d'] = 1;
         $from['M'] = (3 * $quarter) - 2;
         $from['Y'] = $now['year'];

         // next quarter

         $difference = -1;
         $subtractYear = 0;
         $quarter = $quarter - $difference;
         if ($quarter > 4) {
           $now['year'] = $now['year'] + 1;
           $quarter = 1;
         }
         if ($quarter <= 0) {
           $subtractYear = 1;
           $quarter += 4;
         }

         $to['M'] = 3 * $quarter;
         $to['Y'] = $now['year'] - $subtractYear;
         $to['d'] = date('t', mktime(0, 0, 0, $to['M'], 1, $to['Y']));
         $to['H'] = 23;
         $to['i'] = $to['s'] = 59;
       }
     }
