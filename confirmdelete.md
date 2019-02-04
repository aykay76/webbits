This is an example of using Bootstrap within ASP.NET MVC to present a confirmation dialog when deleting something.
To ensure users cannot accidentally delete something due to no confirmation or default OK button, this form presents a dialog where the user must enter a randomly generated code to proceed.

```
@{
    ViewData["Title"] = "Delete Something";
}

<h2>Delete Something</h2>

<form asp-action="DeleteSomething" asp-controller="Management" id="delform">
    <div class="form-group">
        <p>What type of thing do you want to delete?</p>
        <div class="btn-group btn-group-toggle" data-toggle="buttons">
            <label class="btn btn-secondary active">
                <input type="radio" asp-for="Thing" value="this" id="evaluation" autocomplete="off" checked>This
            </label>
            <label class="btn btn-secondary">
                <input type="radio" asp-for="Thing" value="that" id="development" autocomplete="off">That
            </label>
            <label class="btn btn-secondary">
                <input type="radio" asp-for="Thing" value="other" id="production" autocomplete="off">Other
            </label>
        </div>
    </div>

    <div class="form-group">
        <select asp-for="ThingName" class="form-control" id="sources"></select>
    </div>

    <div class="form-group">
        <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#confirmModalCenter" id="delete">Delete The Thing</button>
    </div>
</form>

<div class="modal fade" id="confirmModalCenter" tabindex="-1" role="dialog" aria-labelledby="confirmModalCenterTitle" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="confirmModalLongTitle">Confirm Deletion</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <p>Enter the following code to confirm this action:</p>
                <input type="text" size="10" id="code" />
                <input type="text" size="10" id="usercode" />
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                <button type="button" class="btn btn-primary" id="submit">Confirm Delete</button>
            </div>
        </div>
    </div>
</div>

@section Scripts {
<script>
    $(function() {
        // generate random confirm code
        let code = '';
        for (let i = 0; i < 6; i++)
        {
            let n = Math.random() * 9
            code += Math.floor(n).toString()
        }
        $('#code').val(code)
    });

    $('#usercode').on('change keydown paste input', function() {
        if ($('#usercode').val() == $('#code').val())
        {
            $('#submit').removeAttr("disabled");
        }
        else
        {
            $('#submit').attr("disabled", "disabled");
        }
    });

    $('#delete').click(function() {
        $('#usercode').val('');
        $('#submit').attr("disabled", "disabled");
    });

    $('#submit').click(function() {
        if ($('#usercode').val() == $('#code').val())
        {
            $('#delform')[0].submit();
        }
    });
</script>
}
```
