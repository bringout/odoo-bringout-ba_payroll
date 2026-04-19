# ba_payroll — generated Bosnia-scoped payroll

**This module is GENERATED.** Do not hand-edit. Source is [OCA/payroll](https://github.com/OCA/payroll),
transformed by [`core_0/scripts/rename_oca_payroll.py`](../../../core_0/scripts/rename_oca_payroll.py)
to rename every OCA-owned model with a `ba.` prefix so it coexists
with upstream OCA `payroll` (and Odoo EE `hr_payroll`) in a single database.

## Regenerate

```bash
cd ~/src/bringout/0
python3 core_0/scripts/rename_oca_payroll.py \
  --input  packages/oca-payroll/odoo-bringout-oca-payroll-payroll/payroll \
  --output packages/bringout/odoo-bringout-ba_payroll/ba_payroll \
  --drop-extensions \
  --menu-prefix BA
```

Flags:
- `--drop-extensions` — removes classes that extend core models (hr.contract,
  hr.employee, hr.leave.type) so ba_payroll doesn't fight OCA `payroll` over
  the same shared core tables. Required when both are installed in one DB.
- `--menu-prefix BA` — visual prefix on menu labels so the BA stack is
  distinguishable from OCA `payroll` in the UI. Pass `--menu-prefix ''`
  for production (no visual markers).

## Custom Bosnian logic

**Never** commit hand-edits to `ba_payroll/`. For anything Bosnia-specific
(local salary rules, contribution registers, l10n_ba integration),
create a separate overlay module that depends on `ba_payroll` and extends
its models:

```python
class BaHrPayslip(models.Model):
    _inherit = 'ba.hr.payslip'  # extend the renamed model

    def _compute_contribution_bh(self):
        ...
```

## Rename map (v1 — generated from OCA payroll 16.0.1.6.0)

| OCA model | BA model |
| --- | --- |
| `hr.payslip` | `ba.hr.payslip` |
| `hr.payslip.line` | `ba.hr.payslip.line` |
| `hr.payslip.input` | `ba.hr.payslip.input` |
| `hr.payslip.run` | `ba.hr.payslip.run` |
| `hr.payslip.worked_days` | `ba.hr.payslip.worked_days` |
| `hr.payroll.structure` | `ba.hr.payroll.structure` |
| `hr.salary.rule` | `ba.hr.salary.rule` |
| `hr.salary.rule.category` | `ba.hr.salary.rule.category` |
| `hr.rule.input` | `ba.hr.rule.input` |
| `hr.contribution.register` | `ba.hr.contribution.register` |
| `hr.payslip.employees` | `ba.hr.payslip.employees` (wizard) |
| `hr.payslip.change.state` | `ba.hr.payslip.change.state` (wizard) |
| `payslip.lines.contribution.register` | `ba.payslip.lines.contribution.register` (wizard) |
| `report.payroll.report_payslipdetails` | `report.ba_payroll.report_payslipdetails` |
| `report.payroll.report_contributionregister` | `report.ba_payroll.report_contributionregister` |

Extensions of core models (dropped with `--drop-extensions`):
- `hr.contract` (file `models/hr_contract.py`)
- `hr.employee` (file `models/hr_employee.py`)
- `hr.leave.type` (file `models/hr_leave_type.py`)

When ba_payroll is installed alongside OCA `payroll`, the contract/employee/
leave.type extensions come from OCA's module — accessible on the core tables,
no duplication. If you install ba_payroll **without** OCA payroll, rerun
the generator **without** `--drop-extensions` to get a standalone module.

## License

LGPL-3 (inherited from OCA/payroll, which is LGPL-3 — same license Odoo CE uses).
