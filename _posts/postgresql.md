I'm working with the open payments dataset ( https://openpaymentsdata.cms.gov/ ) . 
The idea is to have all of the data in a single schema which would make it easy for querying . 
I'm learning postgresql while doing this . 

First things first , load the data into postgresql . 

-- Table: public.op_dtl_gnrl_pgyr2015_p01172017

-- DROP TABLE public.op_dtl_gnrl_pgyr2015_p01172017;

CREATE TABLE public.op_dtl_gnrl_pgyr2015_p01172017
(
    id integer NOT NULL DEFAULT nextval('op_dtl_gnrl_pgyr2015_p01172017_id_seq'::regclass),
    change_type character varying(200) COLLATE pg_catalog."default",
    covered_recipient_type character varying(200) COLLATE pg_catalog."default",
    teaching_hospital_ccn integer,
    teaching_hospital_id integer,
    teaching_hospital_name character varying(200) COLLATE pg_catalog."default",
    physician_profile_id integer,
    physician_first_name character varying(200) COLLATE pg_catalog."default",
    physician_middle_name character varying(200) COLLATE pg_catalog."default",
    physician_last_name character varying(200) COLLATE pg_catalog."default",
    physician_name_suffix character varying(200) COLLATE pg_catalog."default",
    recipient_primary_business_street_address_line1 character varying(200) COLLATE pg_catalog."default",
    recipient_primary_business_street_address_line2 character varying(200) COLLATE pg_catalog."default",
    recipient_city character varying(200) COLLATE pg_catalog."default",
    recipient_state character varying(10) COLLATE pg_catalog."default",
    recipient_zip_code character varying(200) COLLATE pg_catalog."default",
    recipient_country character varying(200) COLLATE pg_catalog."default",
    recipient_province character varying(200) COLLATE pg_catalog."default",
    recipient_postal_code character varying(200) COLLATE pg_catalog."default",
    physician_primary_type character varying(200) COLLATE pg_catalog."default",
    physician_specialty character varying(200) COLLATE pg_catalog."default",
    physician_license_state_code1 character varying(10) COLLATE pg_catalog."default",
    physician_license_state_code2 character varying(10) COLLATE pg_catalog."default",
    physician_license_state_code3 character varying(10) COLLATE pg_catalog."default",
    physician_license_state_code4 character varying(10) COLLATE pg_catalog."default",
    physician_license_state_code5 character varying(10) COLLATE pg_catalog."default",
    submitting_applicable_manufacturer_or_applicable_gpo_name character varying(200) COLLATE pg_catalog."default",
    applicable_manufacturer_or_applicable_gpo_making_payment_id bigint,
    applicable_manufacturer_or_applicable_gpo_making_payment_name character varying(200) COLLATE pg_catalog."default",
    applicable_manufacturer_or_applicable_gpo_making_payment_state character varying(200) COLLATE pg_catalog."default",
    applicable_manufacturer_or_applicable_gpo_making_payment_countr character varying(200) COLLATE pg_catalog."default",
    total_amount_of_payment_usdollars numeric,
    date_of_payment date,
    number_of_payments_included_in_total_amount integer,
    form_of_payment_or_transfer_of_value character varying(200) COLLATE pg_catalog."default",
    nature_of_payment_or_transfer_of_value character varying(500) COLLATE pg_catalog."default",
    city_of_travel character varying(200) COLLATE pg_catalog."default",
    state_of_travel character varying(200) COLLATE pg_catalog."default",
    country_of_travel character varying(200) COLLATE pg_catalog."default",
    physician_ownership_indicator character varying(200) COLLATE pg_catalog."default",
    third_party_payment_recipient_indicator character varying(200) COLLATE pg_catalog."default",
    name_of_third_party_entity_receiving_payment_or_transfer_of_val character varying(200) COLLATE pg_catalog."default",
    charity_indicator character varying(200) COLLATE pg_catalog."default",
    third_party_equals_covered_recipient_indicator character varying(200) COLLATE pg_catalog."default",
    contextual_information character varying(1000) COLLATE pg_catalog."default",
    delay_in_publication_indicator character varying(200) COLLATE pg_catalog."default",
    record_id bigint,
    dispute_status_for_publication character varying(200) COLLATE pg_catalog."default",
    product_indicator character varying(200) COLLATE pg_catalog."default",
    name_of_associated_covered_drug_or_biological1 character varying(200) COLLATE pg_catalog."default",
    name_of_associated_covered_drug_or_biological2 character varying(200) COLLATE pg_catalog."default",
    name_of_associated_covered_drug_or_biological3 character varying(200) COLLATE pg_catalog."default",
    name_of_associated_covered_drug_or_biological4 character varying(200) COLLATE pg_catalog."default",
    name_of_associated_covered_drug_or_biological5 character varying(200) COLLATE pg_catalog."default",
    ndc_of_associated_covered_drug_or_biological1 character varying(200) COLLATE pg_catalog."default",
    ndc_of_associated_covered_drug_or_biological2 character varying(200) COLLATE pg_catalog."default",
    ndc_of_associated_covered_drug_or_biological3 character varying(200) COLLATE pg_catalog."default",
    ndc_of_associated_covered_drug_or_biological4 character varying(200) COLLATE pg_catalog."default",
    ndc_of_associated_covered_drug_or_biological5 character varying(200) COLLATE pg_catalog."default",
    name_of_associated_covered_device_or_medical_supply1 character varying(200) COLLATE pg_catalog."default",
    name_of_associated_covered_device_or_medical_supply2 character varying(200) COLLATE pg_catalog."default",
    name_of_associated_covered_device_or_medical_supply3 character varying(200) COLLATE pg_catalog."default",
    name_of_associated_covered_device_or_medical_supply4 character varying(200) COLLATE pg_catalog."default",
    name_of_associated_covered_device_or_medical_supply5 character varying(200) COLLATE pg_catalog."default",
    program_year integer,
    payment_publication_date date
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;

ALTER TABLE public.op_dtl_gnrl_pgyr2015_p01172017
    OWNER to postgres; 
    
    
    
I'm going column by column to see if there is any data that I can remove / combine / modify . 

- change_type 
According to the documentation provided along with the dataset , The Change Type value of NEW indicates that the record is new in the system. 
The Change Type value of ADD indicates that the record is not new in the system, but due to the record not being eligible for publication until the current publication cycle is being published for the first time.
The Change Type value of CHANGED indicates that the record has been previously published but has been modified since its last publication. 
A record whose only change since the last publication is a change to its dispute status is categorized as a changed record.
The Change Type value of UNCHANGED indicates that the record is being republished without change in the current publication. 

A basic count query returned the following output : 
SELECT  change_type,  count(*) as num  FROM public.op_dtl_gnrl_pgyr2015_p01172017 group by change_type ;
(https://github.com/saurabh-rao/saurabh-rao.github.io/blob/master/_posts/images/img1.JPG) ![image](https://github.com/saurabh-rao/saurabh-rao.github.io/blob/master/_posts/images/img1.JPG) ![image]
