deleteSection
=============
@short: 
	deletes a section from  the currently active view (if the opened view isn't Timeline in the 'Tree' mode - the method will be ignored)

@params: 
- section_id	string	the section's id


@returns:
- isSuccess	boolean	returns **true**, if the section was deleted successfully and **false** in other cases (e.g. incorrect section was specified).


@require:treetimeline
@views: timeline
@example: 
scheduler.createTimelineView({
    name:   "timeline",
    render:"tree",
    ...
    y_unit:[
        {key:"production", label:"Production Department", children:[
            {key:"p1", label:"Managers", children:[
                {key:"pm1", label:"John Williams"},
                {key:"pm2", label:"David Miller"}
            ]}
        ]},
        {key:"sales", label:"Sales and Marketing", children:[
            {key:"s1", label:"Kate Moss"},
            {key:"s2", label:"Dian Fossey"}
        ]}
    ]
}); 
...
scheduler.deleteSection("sales");


@template:	api_method
@relatedapi:
	api/scheduler_deleteallsections.md
    api/scheduler_addsection.md
@descr: 

{{note
The method is used for the Tree mode only
}}

@edition:pro