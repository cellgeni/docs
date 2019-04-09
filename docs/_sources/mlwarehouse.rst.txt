
ML Warehouse
============

.. note:: To see the content below you need to be on the Sanger network (either cable or WTSI wifi). Also, we recommend to use either Chrome or Safari browser.

Show me information for all the samples in a study
--------------------------------------------------

.. raw:: html

    <iframe
    src="https://metabase.cellgeni.sanger.ac.uk/public/question/dc1948c5-c121-4619-882c-f1ad24681374"
    frameborder="0"
    width="700"
    height="600"
    allowtransparency></iframe>

MySQL query:

.. code-block:: mysql

    SELECT
        study.id_study_lims,
        study.study_title,
        study.faculty_sponsor,
        study.ethically_approved,
        study.accession_number,
        sample.name, 
        sample.supplier_name,
        sample.donor_id, 
        sample.organism, 
        sample.reference_genome, 
        flowcell.cost_code,
        product_metrics.id_run, 
        flowcell.position, 
        flowcell.tag_index,
        flowcell.tag_sequence, 
        flowcell.tag2_sequence
    
    FROM mlwarehouse.iseq_flowcell as flowcell

    JOIN (mlwarehouse.iseq_product_metrics as product_metrics, 
        mlwarehouse.sample, 
        mlwarehouse.study)

    ON (product_metrics.id_iseq_flowcell_tmp = flowcell.id_iseq_flowcell_tmp 
        AND sample.id_sample_tmp = flowcell.id_sample_tmp 
        AND flowcell.id_study_tmp = study.id_study_tmp)
    
    WHERE study.id_study_lims = 4775

Show me information for all the samples with a cost code
--------------------------------------------------------

.. raw:: html

    <iframe
    src="https://metabase.cellgeni.sanger.ac.uk/public/question/6b6d8fe2-f07f-4a68-b08a-701aefae418c"
    frameborder="0"
    width="700"
    height="600"
    allowtransparency></iframe>

MySQL query:

.. code-block:: mysql

    SELECT 
        study.id_study_lims,
        study.study_title,
        study.faculty_sponsor,
        study.ethically_approved,
        study.accession_number,
        sample.name, 
        sample.supplier_name,
        sample.donor_id, 
        sample.organism, 
        sample.reference_genome, 
        flowcell.cost_code,
        product_metrics.id_run, 
        flowcell.position, 
        flowcell.tag_index,
        flowcell.tag_sequence, 
        flowcell.tag2_sequence
        
    FROM mlwarehouse.iseq_flowcell as flowcell

    JOIN (mlwarehouse.iseq_product_metrics as product_metrics, 
        mlwarehouse.sample, 
        mlwarehouse.study)
    ON (product_metrics.id_iseq_flowcell_tmp = flowcell.id_iseq_flowcell_tmp 
        AND sample.id_sample_tmp = flowcell.id_sample_tmp 
        AND flowcell.id_study_tmp = study.id_study_tmp)

    WHERE flowcell.cost_code = 'S2435'

Show me run ID, lane number and tag index for a sample
------------------------------------------------------

.. raw:: html

    <iframe
    src="https://metabase.cellgeni.sanger.ac.uk/public/question/20bebded-bdc1-4174-8881-476936122eaf"
    frameborder="0"
    width="700"
    height="600"
    allowtransparency></iframe>

MySQL query:

.. code-block:: mysql

    SELECT DISTINCT
        sample.name,
        study.id_study_lims,
        flowcell.pipeline_id_lims,
        product_metrics.id_run,
        flowcell.position,
        flowcell.tag_index,
        flowcell.tag_sequence,
        flowcell.tag2_sequence,
        run_status_dict.description,
        run_status.date

    FROM mlwarehouse.sample

    JOIN (mlwarehouse.iseq_flowcell as flowcell,
        mlwarehouse.iseq_run_status as run_status,
        mlwarehouse.iseq_product_metrics as product_metrics,
        iseq_run_status_dict as run_status_dict,
        mlwarehouse.study as study)

    ON (flowcell.id_sample_tmp = sample.id_sample_tmp
        AND product_metrics.id_iseq_flowcell_tmp = flowcell.id_iseq_flowcell_tmp
        AND run_status.id_run = product_metrics.id_run
        AND run_status.id_run_status_dict = run_status_dict.id_run_status_dict
        AND flowcell.id_study_tmp = study.id_study_tmp)

    WHERE sample.name = 'QC1Hip-11155' AND run_status.iscurrent = 1;
